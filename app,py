from flask import Flask, render_template, redirect, url_for, request, flash
from flask_sqlalchemy import SQLAlchemy
from flask_login import LoginManager, UserMixin, login_user, login_required, logout_user, current_user
from werkzeug.security import generate_password_hash, check_password_hash

app = Flask(__name__)
app.config['SECRET_KEY'] = 'clave_secreta_para_sesiones'
# Conexión a tu XAMPP
app.config['SQLALCHEMY_DATABASE_URI'] = 'mysql+pymysql://root:@localhost/nom_mail'
app.config['SQLALCHEMY_TRACK_MODIFICATIONS'] = False

db = SQLAlchemy(app)

# Configuración de Login
login_manager = LoginManager()
login_manager.init_app(app)
login_manager.login_view = 'login'

# --- MODELOS DE BASE DE DATOS ---
class User(UserMixin, db.Model):
    id = db.Column(db.Integer, primary_key=True)
    username = db.Column(db.String(150), unique=True, nullable=False)
    password = db.Column(db.String(150), nullable=False)

class Moto(db.Model):
    id = db.Column(db.Integer, primary_key=True)
    marca = db.Column(db.String(50), nullable=False)
    modelo = db.Column(db.String(50), nullable=False)
    cilindrada = db.Column(db.Integer, nullable=False)
    imagen = db.Column(db.String(300), nullable=False) # Nuevo campo de imagen

@login_manager.user_loader
def load_user(user_id):
    return User.query.get(int(user_id))

# --- RUTAS DE LA APLICACIÓN ---

@app.route('/')
def index():
    motos = Moto.query.all()
    return render_template('index.html', motos=motos)

@app.route('/about')
def about():
    return render_template('about.html')

@app.route('/register', methods=['GET', 'POST'])
def register():
    if request.method == 'POST':
        username = request.form.get('username')
        password = request.form.get('password')
        
        user_exists = User.query.filter_by(username=username).first()
        if user_exists:
            flash('El nombre de usuario ya está en uso.')
            return redirect(url_for('register'))
            
        hashed_password = generate_password_hash(password, method='pbkdf2:sha256')
        new_user = User(username=username, password=hashed_password)
        db.session.add(new_user)
        db.session.commit()
        
        flash('Registro completado. Ahora puedes iniciar sesión.')
        return redirect(url_for('login'))
    return render_template('register.html')

@app.route('/login', methods=['GET', 'POST'])
def login():
    if request.method == 'POST':
        username = request.form.get('username')
        password = request.form.get('password')
        user = User.query.filter_by(username=username).first()
        
        if user and check_password_hash(user.password, password):
            login_user(user)
            return redirect(url_for('index'))
        else:
            flash('Usuario o contraseña incorrectos.')
    return render_template('login.html')

@app.route('/logout')
@login_required
def logout():
    logout_user()
    return redirect(url_for('index'))

@app.route('/compare', methods=['GET', 'POST'])
@login_required
def compare():
    motos = Moto.query.all()
    moto1 = None
    moto2 = None
    
    if request.method == 'POST':
        moto1_id = request.form.get('moto1')
        moto2_id = request.form.get('moto2')
        
        if moto1_id and moto2_id:
            moto1 = Moto.query.get(moto1_id)
            moto2 = Moto.query.get(moto2_id)
            if moto1_id == moto2_id:
                flash('Por favor, selecciona dos motos diferentes para comparar.')
                
    return render_template('compare.html', motos=motos, moto1=moto1, moto2=moto2)

if __name__ == '__main__':
    with app.app_context():
        db.create_all() 
        
        # Inserta motos de prueba si la tabla está vacía
        if not Moto.query.first():
            motos_ejemplo = [
                Moto(marca='Yamaha', modelo='MT-07', cilindrada=689, imagen='https://images.unsplash.com/photo-1558981403-c5f9899a28bc?w=800&q=80'),
                Moto(marca='Honda', modelo='CB650R', cilindrada=649, imagen='https://images.unsplash.com/photo-1568772585407-9361f9bf3a87?w=800&q=80'),
                Moto(marca='Kawasaki', modelo='Z900', cilindrada=948, imagen='https://images.unsplash.com/photo-1614162692292-7ac56d7f7f1e?w=800&q=80'),
                Moto(marca='Suzuki', modelo='SV650', cilindrada=645, imagen='https://images.unsplash.com/photo-1599819811279-d5ad9cccf838?w=800&q=80')
            ]
            db.session.bulk_save_objects(motos_ejemplo)
            db.session.commit()
            print("Motos de prueba con imágenes insertadas correctamente.")
            
    app.run(debug=True)