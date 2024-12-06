<script>
    import { postData } from '../../api.js'; 
    import { onMount } from 'svelte';

    let idSolicitud = 1; 

   
   
    let filasBacklog = [
        { id: 1, ID_solicitud: idSolicitud, ID_talla: "", BackLog_de_Alto_Nivel: "" },
      
    ]; 

  
  onMount(() => {
   
    let cantidadDeFilas = 1;  

    for (let i = 1; i < cantidadDeFilas; i++) {
      filasBacklog.push({
        id: i + 1, 
        ID_solicitud: 1, 
        ID_talla: "",  
        BackLog_de_Alto_Nivel: ""  
      });
    }
  });
    
  
    
    let opcionesAudiovisual = ["Login de Aplicativo", "Formulario de PQRS", "Registro aplicativo"];
    let opcionesTalla = ["XS", "S", "M", "L", "XL"];
    
    
    let dificultadProyecto = [];
    let calculosTotales = [];

    let estimacionAltoNivel = [
    { talla: "XS", sigla: "XS", puntosMin: 1, puntosMax: 2, pesoPorTalla: 1, descripcion: "Tareas muy pequeñas y simples, requieren poco esfuerzo o tiempo; completadas en horas." },
    { talla: "S", sigla: "S", puntosMin: 3, puntosMax: 5, pesoPorTalla: 2, descripcion: "Tareas pequeñas, requieren algo de análisis o desarrollo; típicamente entre uno o dos días." },
    { talla: "M", sigla: "M", puntosMin: 8, puntosMax: 13, pesoPorTalla: 3, descripcion: "Esfuerzo moderado, de complejidad media; puede tomar de algunos días a una semana." },
    { talla: "L", sigla: "L", puntosMin: 21, puntosMax: 34, pesoPorTalla: 4, descripcion: "Tareas grandes o complejas que requieren significativo esfuerzo; típicamente de una a dos semanas." },
    { talla: "XL", sigla: "XL", puntosMin: 55, puntosMax: 89, pesoPorTalla: 5, descripcion: "Tareas muy grandes y complejas, requieren planificación extensa; suelen tomar varias semanas." }
];

function getColor(talla) {
        switch (talla?.toLowerCase()) {
            case 'xs': return '#5DC1B9';
            case 's': return '#5C8BFF';
            case 'm': return '#4e7699'; 
            case 'l': return '#EA5815';
            case 'xl': return '#8B0000';
            default: return 'transparent';
        }
    }

    let accordions = { ean: false }; 

    function agregarFilaBacklogNueva() {
        filasBacklog = [
            ...filasBacklog,
            {   id: filasBacklog.length + 1,
                ID_solicitud: idSolicitud,  
                ID_talla: "",
                BackLog_de_Alto_Nivel: ""
            }
        ];
    }
    function toggleAccordion(section) {
        accordions[section] = !accordions[section];
    }

    function eliminarFilaBacklog(id) {
        
        filasBacklog = filasBacklog.filter(fila => fila.id !== id);
    }
    export { enviarSolicitud, enviarTallajes, agregarFilaBacklogNueva, eliminarFilaBacklog };
    
   
    async function enviarSolicitud() {
    try {
        const datosSolicitud = {
            tipoProyecto: "Backlog de Alto Nivel", 
            descripcion: "Solicitud inicial para gestionar tallaje de backlog",
            fecha: new Date().toISOString(), 
            creador: "NombreUsuario", 
        };

        const response = await postData('/tallaje', datosSolicitud);

        if (response.success) {
            idSolicitud = response.ID_solicitud; 
            console.log("ID de solicitud guardado:", idSolicitud);
        } else {
            console.error("Error en la respuesta del servidor:", response.error);
        }
    } catch (error) {
        console.error("Error al enviar la solicitud:", error);
    }
}


    function agregarFilaBacklog() {
        filasBacklog = [
            ...filasBacklog,
            {   id: filasBacklog.length + 1,
                ID_solicitud: idSolicitud,  
                ID_talla: "",
                BackLog_de_Alto_Nivel: ""
            }
        ];
    }

    async function enviarTallajes() {
        if (idSolicitud === null) {
            console.error("ID de solicitud no está disponible");
            return;
        }

      
        const datosTallaje = filasBacklog.map(fila => ({
            ID_solicitud: idSolicitud, 
            ID_talla: fila.ID_talla, 
            BackLog_de_Alto_Nivel: fila.BackLog_de_Alto_Nivel 
        }));

        try {
            const response = await postData('/tallaje', datosTallaje);
            if (response.success) {
                console.log("Tallajes enviados correctamente:", response.results);
            } else {
                console.error("Error al enviar tallajes:", response.error);
            }
        } catch (error) {
            console.error("Error al enviar tallajes:", error);
        }
    }



    async function calcularDificultadProyecto() {
    try {
        const response = await postData('/tallaje', filasBacklog);
        console.log("Respuesta del backend:", response);
        
        if (response.success) {
            dificultadProyecto = response.results;
            calculosTotales = response.calculosTotales || {}; 
            
            console.log("Dificultad del Proyecto:", dificultadProyecto);
            console.log("Cálculos Totales:", calculosTotales);
        } else {
            console.error("Error al calcular la dificultad del proyecto:", response.error);
        }
    } catch (error) {
        console.error("Error al calcular la dificultad del proyecto:", error);
    }
}

</script>

  


<style>
    .dificultad-container {
        background-color: #f2f8ff;
        border-radius: 15px;
        width: 80%;
        max-width: 86%;
        padding: 15px;
        box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
        margin-right: 20%;
        margin-bottom: 5%;
        margin-left: 9%;
    }
    .dificultad-container2 {
        background-color: #f2f8ff;
        border-radius: 15px;
        width: 80%;
        max-width: 86%;
        padding: 15px;
        box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
        margin-right: 20%;
        margin-bottom: 10%;
        margin-left: 9%;
    }

    th:nth-child(2), td:nth-child(2) {
        width: 60%;
    }

    th:nth-child(3), td:nth-child(3) {
        width: 40%;
    }

    th:nth-child(4), td:nth-child(4) {
        width: 10%;
    }

    .main-container {
        background-color: #E0E0E0;
        width: 100%;
        height: auto;
        display: flex;
        justify-content: center;
        align-items: center;
        padding: 55px;
        box-sizing: border-box;
        flex-direction: column;
      
        
    }

    .container2 {
        justify-content: space-between;
        width: 80%;
        gap: 10%;
        background-color: #E0E0E0;
        margin-right: 30%;
        margin-bottom: 5%;
        margin-left: 6%;
        
        
    }

    .right {
        width: 80%;
        height: auto;
        max-width: 100%;
        background: #f2f8ff;
        border-radius: 25px;
        padding: 50px;
        box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
        text-align: center;
        
        
    }

    .bottom{
        width: 50%;
        height: auto;
        margin-left: 28%;
        padding: 20px;
        border: 1px solid #ddd;
        border-radius: 20px;
        box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
        background-color: #f2f8ff;
    }

    .header {
        display: flex;
        justify-content: space-between;
        align-items: center;
    }

    .accordion-button {
        padding: 1rem;
        font-size: 1rem;
        margin-left: 0.5rem;
        border: none;
        border-radius: 10px;
        background-color: transparent;
        cursor: pointer;
        transition: color 0.3s;
    }


    .arrow {
        width: 24px;
        height: 24px;
        transition: transform 0.3s ease;
        color: #04132a;
        margin-left: 100px;
    }

    .arrow:hover{
        color: #086ac0;
    }

    .rotate {
        transform: rotate(180deg);
    }

    .title-section {
        font-style: normal;
        font-weight: bold;
        font-size: 25px;
        font-family: "Open Sans", sans-serif;
        color: #04132a;
        margin-bottom: 20px;
    }

    table {
        width: 100%;
        border-collapse: collapse;
    }

    th, td {
        padding: 10px;
        text-align: center;
        border: 1px solid #dddddd;
    }

    input.nombre, .talla {
        width: 60%;
        padding: 8px;
        font-size: 1rem;
        border-radius: 15px;
        border: 1px black solid;
    }

    button.add {
        width: 100px;
        height: 45px;  
        margin-left: 4%;    
        font-weight: bold;
        font-size: 14px;
        cursor: pointer;
        color: #f3ebeb;
        border: none;
        background: linear-gradient(135deg, #086ac0, #1263a5, #1d5688, #5f8cae); 
        border-radius: 25px;
    }

    button.add:hover{
        background: linear-gradient(135deg, #4e7699, #42505b, #2868a0, #086ac0); 
    }

    button.delete{
        width: 50px;
        height: 30px;
        margin: 20px 10px;
        font-weight: bold;
        font-size: 14px;
        cursor: pointer;
        color: #e6dbdb;
        border: none;
        background: linear-gradient(135deg, #99534e, #d51919, #a02828, #6a4242); 
        border-radius: 25px;
    }

    button.delete:hover{
        background: linear-gradient(135deg, #683d3d, #604747, #d51919, #4a1f1f); 
    }

    .button-container {
        display: flex;
        justify-content: center; 
        align-items: flex-end;   
        height: 100%;           
        margin-top: 20px;        
        
    }

    button.send {
        width: 100px;
        height: 45px;  
        margin-left: 13%;    
        font-weight: bold;
        font-size: 14px;
        cursor: pointer;
        color: #f3ebeb;
        border: none;
        background: linear-gradient(135deg, #086ac0, #1263a5, #1d5688, #5f8cae); 
        border-radius: 25px;
    }

    button.send:hover{
        background: linear-gradient(135deg, #4e7699, #42505b, #2868a0, #086ac0); 
    }

    input:hover {
        border: 1px solid #5C8BFF;
        cursor: pointer;
    }



    .information-container-acordion {
    position: relative;
    display: inline-block;
    cursor: pointer;
    }

    .information-text-acordion {
    visibility: hidden;
    width: 250px;
    color: #fff;
    text-align: center;
    border-radius: 35px;
    padding: 1rem;
    position: absolute;
    z-index: 1;
    top: 50%;
    right: 250%;
    transform: translateY(-50%);
    opacity: 0;
    transition: opacity 0.3s;
    }

     .information-container-acordion:hover .information-text-acordion {
    visibility: visible;
    opacity: 1;
    }

    .information-text-acordion::before {
    content: "";
    position: absolute;
    top: 50%;
    left: 100%;
    transform: translateX(50%) scale(1.2);
    border-width: 8px;
    border-style: solid;
    border-color: transparent #000000 transparent transparent;
    border-radius: 50%;
}

    
    .alto-nivel-table {
        width: 100%;
        border-collapse: collapse;
    }
   
      
    .description th:nth-child(2), td:nth-child(2) {
        width: 25%;
    }

    .description th:nth-child(3), td:nth-child(3) {
        width: 25%;
    }

    .description  th:nth-child(4), td:nth-child(4) {
        width: 25%;
    }

    

</style>

<div class="main-container">
    <div class="right">
        <h2 class="title-section">BackLog de Alto Nivel</h2>
        <table>
            <thead>
                <tr>
                    <th>#</th>
                    <th>Requerimiento</th>
                    <th>Talla</th>
                    <th>Eliminar</th>
                </tr>
            </thead>
            <tbody>
                {#each filasBacklog as fila}
                <tr>
                    <td>{fila.id}</td>
                    <td>
                        <input class="nombre" list="nombreOptions" bind:value={fila.BackLog_de_Alto_Nivel} placeholder="Seleccione requerimiento" />
                        <datalist id="nombreOptions">
                            <option value="">Seleccione un nombre</option>
                            {#each opcionesAudiovisual as opcion}
                                <option value={opcion}>{opcion}</option>
                            {/each}
                        </datalist>
                    </td>
                    <td>
                        <input class="talla" list="tallaOptions" bind:value={fila.ID_talla} placeholder="seleccione una talla" />
                        <datalist id="tallaOptions">
                            <option value="">Seleccione una talla</option>
                            {#each opcionesTalla as opcion}
                                <option value={opcion}>{opcion}</option>
                            {/each}
                        </datalist>
                    </td>
                    <td>
                        <button class="delete" on:click={() => eliminarFilaBacklog(fila.id)} title="Eliminar fila">
                            <svg xmlns="http://www.w3.org/2000/svg" class="icon-delete" width="24" height="24" viewBox="0 0 24 24" stroke-width="2" stroke="currentColor" fill="none" stroke-linecap="round" stroke-linejoin="round">
                                <path stroke="none" d="M0 0h24v24H0z" fill="none"/>
                                <path d="M4 7h16" />
                                <path d="M10 11v6" />
                                <path d="M14 11v6" />
                                <path d="M5 7v10a2 2 0 0 0 2 2h10a2 2 0 0 0 2 -2V7" />
                                <path d="M9 7v-2a2 2 0 0 1 2 -2h2a2 2 0 0 1 2 2v2" />
                            </svg>
                        </button>
                    </td>
                </tr>
                {/each}
            </tbody>
        </table>
       
        <div class="button-container">
            <button class="send" on:click={calcularDificultadProyecto}>Enviar</button>
            <button class="add" on:click={agregarFilaBacklog} title="Agregar fila">
                <svg xmlns="http://www.w3.org/2000/svg" class="icon-add" width="24" height="24" viewBox="0 0 24 24" stroke-width="2" stroke="currentColor" fill="none" stroke-linecap="round" stroke-linejoin="round">
                <path stroke="none" d="M0 0h24v24H0z" fill="none"/>
                <path d="M12 4v16m8 -8H4" />
            </svg></button>
        </div>
    </div>
</div>

<div class="container2">
    <div class="bottom">
        <div class="header">
            <h2 class="title-section">Descripción de tallas</h2>
            <button class="accordion-button" type="button" on:click={() => toggleAccordion('ean')}>
                <svg class={`arrow ${accordions.ean ? 'rotate' : ''}`} xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                    <path d="M19 9l-7 7-7-7" />
                </svg>
            </button>
        </div>
        {#if accordions.ean}
        <p>Se hace referencia a la dificultad de la talla. Esta se mostrará cuando el mouse pase sobre la columna de tallas.</p>
        <table class = "description ">
            <thead>
                <tr>
                    <th>Talla</th>
                    <th>Sigla</th>
                    <th>Puntos de Historia Mínimo</th>
                    <th>Puntos de Historia Máximo</th>
                </tr>
            </thead>
            <tbody>
                {#each estimacionAltoNivel as fila}
                <tr>
                    <td>
                        <div class="information-container-acordion">
                            {fila.talla}
                            <div class="information-text-acordion" style="background-color: {getColor(fila.talla)};">
                                {fila.descripcion}
                            </div>
                        </div>
                    </td>
                    <td class="sigla">{fila.sigla}</td>
                    <td>{fila.puntosMin}</td>
                    <td>{fila.puntosMax}</td>
                </tr>
                {/each}
            </tbody>
        </table>
        {/if}
    </div>
</div>

    <div class="dificultad-container">
        <h2 class="title-section">Dificultad del Proyecto por talla</h2>
        <table>
            <thead>
                <tr>
                    <th>Talla</th>
                    <th>Requerimiento</th>
                    <th>Descripción</th>
                    <th>Escenario Optimista</th>
                    <th>Escenario Realista</th>
                    <th>Escenario Negativo</th>
                    <th>Horas Recomendadas</th>
                    <th>Días de Proyecto</th>
                    <th>Semanas de Proyecto</th>
                    <th>Meses de Proyecto</th>
                    
                </tr>
            </thead>
            <tbody>
                {#each dificultadProyecto as item}
                <tr>
                    <td>{item.tallaproyecto}</td>
                    <td>{item.BackLog_de_Alto_Nivel}</td>
                    <td>{item.description}</td>
                    <td>{item.scenarioOptimistic}</td>
                    <td>{item.scenarioRealistic}</td>
                    <td>{item.scenarioPessimistic}</td>
                    <td>{item.horasTotales}</td>
                    <td>{item.diasProyecto}</td>
                    <td>{item.semanasProyecto}</td>
                    <td>{item.mesesProyecto}</td>
                </tr>
                {/each}
            </tbody>
        </table>
    </div>




<div class="dificultad-container2">
    <h2 class="title-section">Dificultad general de tallas</h2>
    <table>
        <thead>
            <tr>
                <th>Talla</th>
                <th>Descripción</th>
                <th>Escenario Optimista</th>
                <th>Escenario Realista</th>
                <th>Escenario Negativo</th>
                <th>Horas Recomendadas</th>
                <th>Días de Proyecto</th>
                <th>Semanas de Proyecto</th>
                <th>Meses de Proyecto</th>
            </tr>
        </thead>
        <tbody>
            {#if calculosTotales.talla_Nombre}
            <tr>
                <td>{calculosTotales.talla_Nombre}</td>
                <td>{calculosTotales.descriptio}</td>
                <td>{calculosTotales.scenarioOptimisti}</td>
                <td>{calculosTotales.scenarioRealisti}</td>
                <td>{calculosTotales.scenarioPessimisti}</td>
                <td>{calculosTotales.horas_Totales}</td>
                <td>{calculosTotales.dias_Proyecto}</td>
                <td>{calculosTotales.semanas_Proyecto}</td>
                <td>{calculosTotales.meses_Proyecto}</td>
            </tr>
            {/if}
        </tbody>
    </table>
</div>

