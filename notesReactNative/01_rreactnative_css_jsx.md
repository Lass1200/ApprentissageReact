# React Native — CSS & JSX

---

## CSS inline et StyleSheet

```jsx
import React from 'react'
import { View, Text, StyleSheet } from 'react-native'

function App() {
  return (
    <View style={styles.wrapper}>
      <View style={styles.view1}>
        <Text style={styles.textOne}>daz
          <Text>bold</Text>
        </Text>
        <Text style={styles.textOne}>2ad</Text>
      </View>
      <View style={styles.view2}>
        <Text style={styles.textOne}>3daz</Text>
        <Text style={styles.textOne}>4dazdddd</Text>
      </View>
    </View>
  )
}

const styles = StyleSheet.create({
    wrapper: {
      marginTop: 100,
      flexDirection: "column"
    },
    view1: {
      backgroundColor: "green",
    },
    view2: {
      backgroundColor: "purple"
    },
    textOne: {
      fontFamily: "Cochin",
      fontWeight: "bold"
    }
})

export default App
```

---

## JSX dans React Native

```jsx
import React from 'react'
import { View, Text, StyleSheet } from 'react-native'

const App = () => {
  const getNames = (firstName, secondName, thirdName) => {
    return firstName + secondName + thirdName
  }

  return (
    <Text style={styles.wrapper}>
      {/* ou backtick */}
      {getNames("stan", "steve", "roger")}
    </Text>
  )
}

const styles = StyleSheet.create({
    wrapper: {
      marginTop: 100,
      flexDirection: "column"
    },
    view1: {
      backgroundColor: "green",
    },
    view2: {
      backgroundColor: "purple"
    },
    textOne: {
      fontFamily: "Cochin",
      fontWeight: "bold"
    }
})

export default App
```
