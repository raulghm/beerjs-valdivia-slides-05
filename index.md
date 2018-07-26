title: Programación funcional en JavaScript
author:
  name: Raúl Hernández
  twitter: raulghm
  url: https://github.com/raulghm
output: index.html
controls: true
style: style.css
theme: juanbrujo/cleaver-beerjs

--

# Programación funcional en JavaScript
## Introducción y casos prácticas

--

# ¿FP?

## Functional programming

> Functional programming (often abbreviated FP) is the process of building software...

--

# Algunos conceptos

* Es otro paradigma (~OOP, ~PP)
* Las funciones pueden ser valores
* Es declarativa
* Es otra forma de pensar y enfrentar la lógica
* El código es mas preciso, mas legible, mas fácil manipular 
* Funciones de primer orden
* Bugs

--

# Ejemplo #1

```js
const suma = (a, b) => {
  return a + b
}

console.log(sum(1, 2))
```

--

# Ejemplo #2

```js
const suma = (a, b) => {
  return a + b
}

const calculo = suma
const resultado = calculo(1, 2)

console.log(resultado) // 3
```

--

# Ejemplo #3

```js
// Datos
const datos = [
  {
    id: 0,
    name: 'Gustavo',
    job: 'backend'    
  },
  {
    id: 1,
    name: 'Juan',
    job: 'designer'
  },
  {
    id: 2,
    name: 'Raúl',
    job: 'frontend'
  }
]
```

--

# Opción 1

```js
// Opcion #1 - Guardar en un array solo los usuarios de tipo backend
let backend = []

for (i = 0; i < datos.length; i++) {
  if (datos[i].job === 'backend') {
    backend.push(datos[i])
  }
}

console.log({backend}) // [{ id: 0, name: 'Gustavo', job: 'backend'...
```

--

# Opción 2

```js
/* Opcion #2 - Guardar en un array solo los usuarios de tipo backend
de forma funcional */

const backend2 = datos.filter(function(user) {
  return user.job === 'backend'
})

console.log({backend2}) // [{ id: 0, name: 'Gustavo', job: 'backend'...

```

--

# Opción 3

```js
// Opcion #3 - Ó en una sola linea
const backend3 = datos.filter(user => user.job === 'backend')

console.log({backend3}) // [{ id: 0, name: 'Gustavo', job: 'backend'...
```

--

# Opción 4

```js
// Opcion #4 - De forma compuesta
const filtro = user => user.job === 'backend'
const backend4 = datos.filter(filtro)

console.log({backend4}) // [{ id: 0, name: 'Gustavo', job: 'backend'...
```

https://repl.it/@raulghm/WindingGoldShareware

--

# Map + reduce

```js
// Datos
const productos = [
  {
    name: 'Palta',
    price: 3800
  },
  {
    name: 'Lechuga',
    price: 400
  },
  {
    name: 'Limon',
    price: 800
  }
]
```

--

```js
const resultado = productos
                  .map(item => item.price)
                  .reduce((suma, producto) => suma + producto)

console.log({resultado}) // 5000
```

https://repl.it/@raulghm/FragrantMajorProfiles

--

# Map, reduce, filter, find

### Map
Extra y crear un nuevo arreglo

### Filter
Extrae todos los resultados que encuentra

### Find
Extrae el primer valor que encuentra

### Reduce
Aplica un acumulador a cada elemento de izquierda a derecha, reduciendo en un solo valor

https://developer.mozilla.org/es/docs/Web/JavaScript/Referencia/Objetos_globales/Array/prototype

--

# Promesas + async/await

--

# Opción 1

```js
const promesa = (value) => {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      if (value > 0) {
        resolve(true)
      } else {
        reject(false)
      }
    }, 200)
  })
}

promesa(0).then(value => {
  console.log(value)
}).catch(value => {
  console.log(value)
})
```

--

# Opción 2

```js
const promesa = (value) => {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      if (value > 0) {
        resolve(true)
      } else {
        reject(false)
      }
    }, 600)
  })
}

const respuesta = async value => {
  const res = await promesa(value)
  console.log(res)
}

console.log(respuesta(1))
```

https://repl.it/@raulghm/NuttyRoyalblueAddress

--

# Consejos FP en JavaScript

* Evitar librerias de terceros como underscore o lodash (si no es extrictamente necesario)
* Aprovechar los prototipos de ES6 para exprimir el código https://developer.mozilla.org/es/docs/Web/JavaScript/Referencia/Objetos_globales/Array/prototype

* Siempre documentar http://usejsdoc.org y testear
* En React prefiere componentes funcionales versus componentes como clases

--

# Bonus
## Componentes en React

--

# Clases

```js
import React, { Component } from 'react'
import PropTypes from 'prop-types'
import { View } from 'react-native'
import styled from 'styled-components'
import Icon from 'react-native-vector-icons/Ionicons'
import { Colors, Metrics } from '../themes'

/** @component */
class Checkbox extends Component {
  constructor (props) {
    super(props)
  }

  static propTypes = {
    isActive: PropTypes.bool
  }

  render () {
    const { isActive } = this.props

    const Wrapper = styled(View)`
      border-radius: ${Metrics.borderRadius.base};
      border: 2px solid ${Colors.grayLight};
      background-color: ${props => props.isActive ? Colors.grayLight : Colors.white};
      width: 30px;
      height: 30px;
      align-items: center;
    `

    return (
      <Wrapper {...this.props}>
        {isActive && (
          <Icon
            name="md-checkmark"
            size={22}
            color={Colors.white}
            style={{marginTop: 2}}
          />
        )}
      </Wrapper>
    )
  }
}

export default Checkbox
```

--

# Componentes Funcionales
## (stateless component)

```js
import React, { Component } from 'react'
import PropTypes from 'prop-types'
import { View } from 'react-native'
import styled from 'styled-components'
import Icon from './Icon'
import { Colors, Metrics } from '../themes'

/** @component */
const Checkbox = props => {
  const { isActive } = props

  const Wrapper = styled(View)`
    border-radius: ${Metrics.borderRadius.base};
    border: 2px solid ${Colors.grayLight};
    background-color: ${props => props.isActive ? Colors.grayLight : Colors.white};
    width: 30px;
    height: 30px;
    align-items: center;
  `

  return (
    <Wrapper {...props}>
      {isActive && (
        <Icon
          name="md-checkmark"
          size={22}
          color={Colors.white}
          style={{marginTop: 2}}
        />
      )}
    </Wrapper>
  )
}

Checkbox.PropTypes = {
  /** Name */
  isActive: PropTypes.bool
}

Checkbox.defaultProps = {
  isActive: false
}

export default Checkbox

```
--

# Gracias