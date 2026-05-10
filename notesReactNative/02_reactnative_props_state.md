# React Native — Props & State

---

## Props

```jsx
import React from 'react'
import { View, Text, StyleSheet } from 'react-native'

// ou destructuring
const Person = (props) => {
  return (
    <View>
      {
        props.age ? (
          <Text style={styles.textOne}>je suis : {props.name} et j'ai {props.age}</Text>
        ) : (
          <Text style={styles.textOne}>je suis : {props.name}</Text>
        )
      }
    </View>
  )
}

const App = () => {
  return (
    <View style={styles.wrapper}>
      <Person name="rroger" />
      <Person name="stan" />
      <Person name="r" age='14' />
    </View>
  )
}

const styles = StyleSheet.create({
  wrapper: {
    marginTop: 100,
    flexDirection: "column"
  },
  textOne: {
    fontFamily: "Cochin",
    fontWeight: "bold"
  }
})

export default App
```

---

## Props children

Le contenu placé entre les balises d'un composant est accessible via `props.children`.

```jsx
import React from 'react'
import { View, Text, StyleSheet } from 'react-native'

const Person = (props) => {
  return (
    <View>
      {
        props.age ?
          <Text style={styles.textOne}>je suis : {props.name} et j'ai {props.age} et {props.children}</Text>
          :
          <Text style={styles.textOne}>je suis : {props.name}</Text>
      }
    </View>
  )
}

const App = () => {
  return (
    <View style={styles.wrapper}>
      <Person name="rroger">Alien</Person>
      <Person name="stan">human</Person>
      <Person name="r" age='14'>human</Person>
    </View>
  )
}

const styles = StyleSheet.create({
  wrapper: {
    marginTop: 100,
    flexDirection: "column"
  },
  textOne: {
    fontFamily: "Cochin",
    fontWeight: "bold"
  }
})

export default App
```

---

## State dans React Native

Pour des choses un peu plus interactives, le state permet de stocker des données propres à son composant.

Si le props c'est pour le rendu statique venant du parent → le state c'est pour les données qui changent dans le composant lui-même.
