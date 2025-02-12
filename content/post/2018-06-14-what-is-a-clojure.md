+++
title = "¿Qué es una clausura?"
author = ["Sergio Benítez"]
description = "No entender las clausuras en JavaScript es como intentar hablar inglés sin conocer la grámatica del idioma."
date = 2018-06-14T00:00:00-05:00
tags = [
  "javascript",
]
+++

# Table of Contents

1.  [Aprenderás](#org39efdb8)
2.  [¿Qué es una clausura?](#org92e6193)
3.  [Usando clausuras](#orgf03b541)
    1.  [Privacidad de datos](#org651d88a)
    2.  [Funciones con estado](#org10b204d)
    3.  [Programación funcional](#org488123b)
4.  [Puntos clave](#org8869c25)

Las clausuras (*clojures* en inglés) son importantes en JavaScript porque son quienes controlan que esta y que no esta dentro del alcance de una función en particular, junto con variables que son compartidas entre funciones hermanas que se encuentran en un mismo ámbito.

Entender cómo estas variables y funciones se relacionan es crítico para el entendimiento del código en JavaScript en ambos estilos de programación; programación funcional y programación orientada a objetos.

El no tener conocimiento de como las clausuras funcionan, es una bandera roja que puede revelar falta de experiencia no solo en JavaScript, sino en cualquier lenguaje que se basa en clausuras (e.g., Haskell, C##, Python, etc&#x2026;).

Las clausuras son **usadas con frecuenca en JavaScript** para la privacidad de datos en objetos, en manejadores de eventos, funciones de respaldo, aplicaciones parciales, currying y otros patrones de programación funcional.

# Aprenderás {#org39efdb8}

-   ✅ ¿Qué es una clausura?
-   ✅ Clausuras en privacidad de datos
-   ✅ Clausuras en aplicaciones parciales


# ¿Qué es una clausura? {#org92e6193}

Por definición se tiene:

> Un clausura es la combinación de una función agrupada (cerrada) con referencias a su estado circundante (el entorno léxico)

El concepto técnico *entorno léxico* puede ser engorroso; no obstante, se puede entender una clausura como un acceso hacia el alcance de una función externa desde una función interna. La siguiente es la estructura básica de una clausura:

    const outerFn = (outerFnParam) => {
      return {
        innerFn = () => outerFnParam
      }
    }

En JavaScript, las clausuras son creadas cada vez que una función es creada.

Para usar una clausura, se debe definir una función dentro de otra función y exponerla. Para exponer una función, se retorna la función ó se pasa como parámetro de otra función.

La función interior tendrá acceso a las variables en el ámbito de la función externa, incluso después de que la función externa se haya retornado.

# Usando clausuras {#orgf03b541}

Entre otras cosas, las clausuras se usan comunmente para darle a los objetos privacidad de datos. La privacidad de datos es una propiedad esencial para programar una interfaz y no una implementación. El concepto es importante para construir software robusto porque los detalles de implementación son más propensos a cambiar y romper código que los contratos entre interfaces.

Por otra parte, en programación funcional las clausuras se usan con frequencia para aplicaciones parciales y currying, conceptos que revisaremos en su debida sección.


## Privacidad de datos {#org651d88a}

En JavaScript, las clausuras son el mecanismo principal utilizado para permitir la privacidad de los datos. Con clausuras, las variables adjuntas solo están en alcance dentro de la función externa que contiene dichas variables. No puedes obtener los datos de un alcance exterior salvo que se usen los métodos privilegiados de objetos. En JavaScript, cualquier método expuesto definido dentro del alcance de una clausura es privilegiado. El siguiente código nos ilustra el concepto de métodos privilegiados:

    const getSecret = (secret) => {
      return {
        get: () => secret
      };
    };
    
    test('Closure for object privacy.', assert => {
      const msg = '.get() should have access to the closure.';
      const expected = 1;
      const obj = getSecret(1);
    
      const actual = obj.get();
    
      try {
        assert.ok(secret, 'This throws an error.');
      } catch (e) {
        assert.ok(true, `The secret var is only available
          to privileged methods.`);
      }
    
      assert.equal(actual, expected, msg);
      assert.end();
    });

Aquí, el método `.get()` es definido dentro del alcance de `.getSecret()`, lo que le da acceso a cualquier variable en `getSecret()` y lo convierte en un método privilegiado. En este caso puntual, el parámetro `secret` es un método privilegiado.

## Funciones con estado {#org10b204d}

Los objetos no son la única manera de producir privacidad de datos. Las clausuras también se pueden utilizar para crear funciones con estado cuyos valores de retorno pueden estar influenciados por su estado interno. Revisemos el siguiente código:

    const secret = (msg) => () => msg;
    
    test('secret', assert => {
      const msg = 'secret() should return a function that returns the passed secret.';
    
      const theSecret = 'Closures are easy.';
      const mySecret = secret(theSecret);
    
      const actual = mySecret();
      const expected = theSecret;
    
      assert.equal(actual, expected, msg);
      assert.end();
    });

Para este caso puntual, la función interna sólo retorna el valor que recibio del parámetro `msg` de la función externa. No obstante, la función interna esta en capacidad de modificar el valor del parámetro `msg`.

## Programación funcional {#org488123b}

Para explorar el uso de clausuras en programación funcional se requieren las siguientes definiciones:

> **Aplicación:** Es el proceso de *aplicar* una función a sus argumentos con el proposito de producir un valor de retorno

> **Aplicación Parcial:** Es el proceso de *aplicar* una función a *algunos de sus argumentos*. La función parcialmente aplicada se devuelve para un posterior uso sobre uno o más argumentos dentro de la función devuelta, y la función devuelta toma los parámetros restantes como argumentos para completar la aplicación de función.

La definición de la aplicación parcial es engorrosa, pero con el concepto de clausuras se mejora su entendimiento.La aplicación parcial aprovecha el alcance de la clausura para **fijar** parámetros. Puede escribir una función genérica que aplicará parcialmente argumentos a la función de destino. La siguiente firma corresponde a una aplicación parcial:

    partialApply(targetFunction: Function, ...fixedArgs: Any[]) =>
      functionWithFewerParams(...remainingArgs: Any[])

En esta firma tenemos que la aplicación parcial tomará una función que tome cualquier número de argumentos, seguido de argumentos que queremos aplicar parcialmente a la función, y devuelve una función que tomará los argumentos restantes. El siguiente ejemplo ilustra un caso de uso de aplicación parcial. Digamos que hay una función que suma 2 números:

    const add = (a, b) => a + b;

Ahora, se quiere una función que sume 10 a cualquier número. Dicha función se llamará `add10()`. El resultado de `add10(5)` debería ser `15`. Una función `partialApply()` hace que esto sea posible de la siguiente manera:

    cosnt add10 = partialApply(add, 10);
    add10(5);

Aquí, el argumento `10` se convierte en un parámetro fijo que será recordado dentro del alcance de la clausura de la función `add10()`.

Quedaría pendiente una implementación de la función `partialApply()`. El siguiente código es una posible implementación de dicha función:

    const partialApply = (fn, ...fixedArgs) => {
      return function (...remainingArgs) {
        return fn.apply(this, fixedArgs.concat(remainingArgs));
      };
    };
    
    
    test('add10', assert => {
      const msg = 'partialApply() should partially apply functions'
    
      const add = (a, b) => a + b;
    
      const add10 = partialApply(add, 10);
    
    
      const actual = add10(5);
      const expected = 15;
    
      assert.equal(actual, expected, msg);
    });

Como puedes ver, en la implementación simplemente se devuelve una función que conserva el acceso a los argumentos de `FixedArgs` que se pasaron a la función `partialApply()` .

# Puntos clave {#org8869c25}

-   Una clausura permite un acceso hacia el alcance de una función externa desde una función interna.
-   Las clausuras permiten manejar privacidad de datos en objetos.
-   La privacidad de datos es una propiedad esencial para programar una interfaz y no una implementación
-   Las clausuras permiten manejar funciones con estado.
-   Las clausuras habilita aplicaciones parciales en JavaScript.

