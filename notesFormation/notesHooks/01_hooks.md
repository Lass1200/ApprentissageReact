# Hooks

---

## C'est quoi

Les hooks = fonctions qui permettent de bénéficier de certaines fonctionnalités React (state, cycle de vie...) sans avoir besoin des classes.

Plus besoin des classes. Commence toujours par `use`.

---

## Différence classe vs function component

```jsx
// Classe
import React, { Component } from 'react'

class ClassState extends Component {
    constructor(props) {
        super(props)
        this.state = { counter: 0 }
    }

    addOne = () => {
        this.setState(prevState => ({ counter: prevState.counter + 1 }))
    }

    render() {
        return (
            <div>
                <p>class state: {this.state.counter}</p>
                <button onClick={this.addOne}>state dans class</button>
            </div>
        )
    }
}

export default ClassState
```

```jsx
// Function component — équivalent moderne
const FunctionState = () => {
    const [counter, setCounter] = useState(0)

    // inline directement dans onClick aussi faisable
    const addOne = () => {
        setCounter(prevCounter => prevCounter + 1)
    }

    return(
        <div>
            <p>function state {counter}</p>
            <button onClick={addOne}>state dans une fonction</button>
        </div>
    )
}

export default FunctionState
```

---

## useState

```jsx
const [valeur, setValeur] = useState(valeurInitiale)
//     👆 lire   👆 modifier       👆 valeur de départ
```

Règle importante — on ne modifie jamais directement la valeur, on passe toujours par le setter :

```jsx
todos.push(...)    // ❌ React ne voit pas le changement
setTodos([...])    // ✅ React re-rend le composant
```

Ne jamais oublier le setter :

```jsx
const [todos] = useState([])           // ❌
const [todos, setTodos] = useState([]) // ✅
```

### Le spread `...todos`

```jsx
setTodos([...todos, { id: Date.now(), todo: newTodo }])
//        👆 copie tous les anciens todos + ajoute le nouveau
```

On ne peut pas faire `todos.push()` avec React, donc on crée un nouveau tableau avec tous les anciens éléments + le nouveau.

### Le `&&` conditionnel

```jsx
// Pattern très courant en React
const msg = warning && <div>alerte !</div>
//          👆 si warning est true → affiche le div
//             si warning est false → n'affiche rien
```

### Exemple todo app complet

```jsx
// Todo.js
import { useState } from "react"
import AddToDoForm from "./AddTodoForm"

const Todo = () => {
    const [todos, setTodos] = useState([
        { id: 1, todo: "acheter lait" },
        { id: 2, todo: "acheter du pain" },
        { id: 3, todo: "acheter du pain" }
    ])

    const [warning, setWarning] = useState(false)

    const myTodos = todos.map((todo) => {
        return (
            <li className="list-group-item" key={todo.id}>{todo.todo}</li>
        )
    })

    const addNewToDo = (newTodo) => {
        if (newTodo !== '') {
            warning && setWarning(false)
            setTodos([...todos, {
                id: Date.now(),
                todo: newTodo
            }])
        } else {
            setWarning(true)
        }
    }

    const msg = warning && <div className="alert alert-dark mt-5" role="alert">
        champ vide !
    </div>

    return (
        <div>
            {msg}
            <h1 className="text-center">{todos.length} to-do</h1>
            <ul className="list-group">
                {myTodos}
            </ul>
            <AddToDoForm addNewToDo={addNewToDo} />
        </div>
    )
}

export default Todo
```

```jsx
// AddToDoForm.js
import { useState } from "react"

const AddToDoForm = (props) => {
    const [addTodo, setAddTodo] = useState('')

    const handleToDo = (e) => {
        e.preventDefault()
        props.addNewToDo(addTodo)
        setAddTodo("")
    }

    return (
        <form className="mt-4" onSubmit={handleToDo}>
            <div className="card card-body">
                <div className="form-group">
                    <label>ajouter todo</label>
                    <input
                        className="form-control"
                        type="text"
                        value={addTodo}
                        onChange={(e) => setAddTodo(e.target.value)}
                    />
                    <input className="btn btn-success mt-4" type="submit" />
                </div>
            </div>
        </form>
    )
}

export default AddToDoForm
```

---

## useEffect

Combinaison entre les méthodes de cycle de vie. Remplace `componentDidMount`, `componentDidUpdate` et `componentWillUnmount`.

```jsx
import React from 'react'
import { useState, useEffect } from 'react'

function FunctionCount() {
    const [count, setCount] = useState(0)

    useEffect(() => {
        setTimeout(() => {
            document.title = `vous avez cliqué ${count}`
        }, 5000)
    })

    return (
        <div>
            <p>{count}</p>
            <button onClick={() => setCount(count + 1)}>cliquer</button>
        </div>
    )
}

export default FunctionCount
```

### Le tableau de dépendances

```jsx
useEffect(() => { ... })           // sans [] → s'exécute à chaque render
useEffect(() => { ... }, [])       // [] → une seule fois au montage (= componentDidMount)
useEffect(() => { ... }, [count])  // [count] → à chaque fois que count change (= componentDidUpdate)
```

---

## Équivalences class → hooks

| Class component | Hook équivalent |
|---|---|
| `this.state` / `setState` | `useState()` |
| `componentDidMount` | `useEffect(() => {}, [])` |
| `componentDidUpdate` | `useEffect(() => {}, [dep])` |
| `componentWillUnmount` | `useEffect(() => { return () => cleanup }, [])` |
| `this.props.match.params` | `useParams()` |
| `props.history.push` | `useNavigate()` |
