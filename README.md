# To-do-list
This code creates a list of tasks with interaction in the console and in an HTML environment with buttons to complete and delete tasks.

let tareas = [];

function agregarTarea(descripcion) {
    if (descripcion.trim() === "") {
        console.log("La descripción no puede estar vacía.");
        return;
    }
    const tarea = {
        id: Date.now(),
        descripcion,
        completada: false
    };
    tareas.push(tarea);
    mostrarTareas();
}

function eliminarTarea(id) {
    tareas = tareas.filter(tarea => tarea.id !== id);
    mostrarTareas();
}

function completarTarea(id) {
    tareas = tareas.map(tarea =>
        tarea.id === id ? { ...tarea, completada: !tarea.completada } : tarea
    );
    mostrarTareas();
}


function mostrarTareas() {
    console.clear();
    console.log("Lista de Tareas:");
    tareas.forEach(tarea => {
        console.log(`${tarea.completada ? "✔" : "❌"} ${tarea.descripcion} (ID: ${tarea.id})`);
    });
}

agregarTarea("Aprender JavaScript");
agregarTarea("Practicar algoritmos");
agregarTarea("Desarrollar un proyecto");

setTimeout(() => completarTarea(tareas[0].id), 2000);
setTimeout(() => eliminarTarea(tareas[1].id), 4000);


document.addEventListener("DOMContentLoaded", () => {
    const inputTarea = document.getElementById("tarea");
    const btnAgregar = document.getElementById("agregar");
    const listaTareas = document.getElementById("lista");

    btnAgregar.addEventListener("click", () => {
        agregarTarea(inputTarea.value);
        inputTarea.value = "";
    });

    function actualizarLista() {
        listaTareas.innerHTML = "";
        tareas.forEach(tarea => {
            const li = document.createElement("li");
            li.textContent = tarea.descripcion;
            if (tarea.completada) li.style.textDecoration = "line-through";
            
            const btnCompletar = document.createElement("button");
            btnCompletar.textContent = "✔";
            btnCompletar.onclick = () => completarTarea(tarea.id);
            
            const btnEliminar = document.createElement("button");
            btnEliminar.textContent = "❌";
            btnEliminar.onclick = () => eliminarTarea(tarea.id);
            
            li.appendChild(btnCompletar);
            li.appendChild(btnEliminar);
            listaTareas.appendChild(li);
        });
    }
    setInterval(actualizarLista, 1000);
});
