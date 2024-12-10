<script>
    import { onMount } from 'svelte';
    import { goto } from '$app/navigation';
    import { browser } from '$app/environment';
    
    // Estado para controlar el slider
    let isSignUpMode = false;
    let isLoading = false;
    let generalError = '';
    
    // Variables para el login
    let loginEmail = '';
    let loginPassword = '';
    let rememberMe = true;
    let showLoginPassword = false;
    
    // Variables para el registro
    let registerName = '';
    let registerLastName = '';
    let registerEmail = '';
    let registerPassword = '';
    let registerConfirmPassword = '';
    let showRegisterPassword = false;
    
    // Variables de error
    let errorMail = '';
    let errorPassword = '';
    let errorConfirmPassword = '';

    onMount(() => {
        // Limpiar errores al montar el componente
        clearErrors();
    });

    function clearErrors() {
        errorMail = '';
        errorPassword = '';
        errorConfirmPassword = '';
        generalError = '';
    }

    // Función para cambiar entre login y registro
    const toggleMode = () => {
        clearErrors();
        isSignUpMode = !isSignUpMode;
    };

    // Validaciones
    function validarEmail(email) {
        try {
            const emailPattern = /^[a-zA-Z0-9._-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,4}$/;
            if (!email) {
                errorMail = 'Debes introducir un email';
                return false;
            } else if (!emailPattern.test(email)) {
                errorMail = 'El email debe ser válido';
                return false;
            }
            errorMail = '';
            return true;
        } catch (error) {
            console.error('Error en validación de email:', error);
            errorMail = 'Error al validar el email';
            return false;
        }
    }

    function validarPassword(password) {
        try {
            if (!password) {
                errorPassword = 'Debes introducir una contraseña';
                return false;
            } else if (password.length < 6) {
                errorPassword = 'La contraseña debe tener al menos 6 caracteres';
                return false;
            }
            errorPassword = '';
            return true;
        } catch (error) {
            console.error('Error en validación de contraseña:', error);
            errorPassword = 'Error al validar la contraseña';
            return false;
        }
    }

    async function handleLogin() {
        try {
            clearErrors();
            isLoading = true;

            if (!validarEmail(loginEmail) || !validarPassword(loginPassword)) {
                return;
            }

            // Aquí iría tu lógica de login
            console.log('Login válido');
            
            // Simular llamada al servidor
            await new Promise(resolve => setTimeout(resolve, 1000));
            
            // Redirigir después del login exitoso
            if (browser) {
                goto('/dashboard');
            }

        } catch (error) {
            console.error('Error en login:', error);
            generalError = 'Error al iniciar sesión. Por favor, intenta nuevamente.';
        } finally {
            isLoading = false;
        }
    }

    async function handleRegister() {
        try {
            clearErrors();
            isLoading = true;

            if (!validarEmail(registerEmail) || 
                !validarPassword(registerPassword)) {
                return;
            }

            if (registerPassword !== registerConfirmPassword) {
                errorConfirmPassword = 'Las contraseñas no coinciden';
                return;
            }

            // Aquí iría tu lógica de registro
            console.log('Registro válido');
            
            // Simular llamada al servidor
            await new Promise(resolve => setTimeout(resolve, 1000));
            
            // Redirigir después del registro exitoso
            if (browser) {
                goto('/dashboard');
            }

        } catch (error) {
            console.error('Error en registro:', error);
            generalError = 'Error al registrarse. Por favor, intenta nuevamente.';
        } finally {
            isLoading = false;
        }
    }
</script>

<style>
    .container {
        display: grid;
        grid-template-columns: 1fr 1fr;
        min-height: 100vh;
        background: #fff;
        margin-left: 17%;
        margin-right: 17%;
        margin-top: 5%;
        margin-bottom: 5%;
        border-radius: 5%;
    }

    .form-container {
        display: flex;
        flex-direction: column;
        justify-content: center;
        padding: 0 4rem;
        width: 80%;
        
    }

    .title {
        font-size: 2.5rem;
        color: #333;
        margin-bottom: 2rem;
    }

    .input-field {
        width: 100%;
        height: 50px;
        background-color: #f5f5f5;
        margin: 0.5rem 0;
        border-radius: 8px;
        display: flex;
        align-items: center;
    }

    .input-field input {
        background: none;
        outline: none;
        border: none;
        line-height: 1;
        font-weight: 500;
        font-size: 1.1rem;
        color: #333;
        width: 100%;
        padding: 0 1rem;
    }

    .btn {
        width: 150px;
        height: 45px;
        border: none;
        outline: none;
        border-radius: 25px;
        cursor: pointer;
        background:linear-gradient(135deg, #086ac0, #1263a5, #1d5688, #5f8cae); 
        color: #fff;
        text-transform: uppercase;
        font-weight: 600;
        margin: 1rem 0;
        transition: .3s;
        
    }

    .btn:hover {
        background: linear-gradient(135deg, #4e7699, #42505b, #2868a0, #086ac0); 
        transition: all .3s;
    }

    .info-container {
        background:linear-gradient(135deg, #086ac0, #1263a5, #1d5688, #5f8cae); 
        color: #fff;
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: center;
        text-align: center;
        padding: 0 2rem;
        border-radius: 5%;
    }

    .info-container h3 {
        font-size: 2.2rem;
        margin-bottom: 1rem;
       
    }

    .info-container p {
        font-size: 1.1rem;
        margin-bottom: 2rem;
        border-radius: 10%;
    }

    .btn.transparent {
        background: none;
        border: 2px solid #fff;
        width: 130px;
        height: 41px;
        font-weight: 600;
        font-size: 0.8rem;
    }
</style>

<div class="container">
    {#if !isSignUpMode}

    <div class="info-container">
        <h3>¿Eres nuevo aquí?</h3>
        <p>Registrate y unete a la familia linktic</p>
        <button class="btn transparent" on:click={toggleMode}>
            REGISTRARSE
        </button>
    </div>
        <!-- Formulario de Login -->
        <div class="form-container">
            <h2 class="title">Iniciar Sesión</h2>
            <div class="input-field">
                <input type="email" placeholder="Email" bind:value={loginEmail} />
            </div>
            <div class="input-field">
                <input type="password" placeholder="Contraseña" bind:value={loginPassword} />
            </div>
            <button class="btn">INICIAR SESIÓN</button>
        </div>
    
    {:else}
        <!-- Formulario de Registro -->
        <div class="form-container">
            <h2 class="title">Registrarse</h2>
            <div class="input-field">
                <input type="text" placeholder="Nombre" bind:value={registerName} />
            </div>
            <div class="input-field">
                <input type="text" placeholder="Apellido" bind:value={registerLastName} />
            </div>
            <div class="input-field">
                <input type="email" placeholder="Email" bind:value={registerEmail} />
            </div>
            <div class="input-field">
                <input type="password" placeholder="Contraseña" bind:value={registerPassword} />
            </div>
            <button class="btn">REGISTRARSE</button>
        </div>
        <!-- Panel Derecho -->
        <div class="info-container">
            <h3>¿Ya eres uno de nosotros?</h3>
            <p>Inicia sesión y continúa tu experiencia</p>
            <button class="btn transparent" on:click={toggleMode}>
                INICIAR SESIÓN
            </button>
        </div>
    {/if}
</div>
  