# Kotlin-Flows

## Documentation

* [Singleton instantiation](#singleton-instantiation)


## Singleton instantiation

```Kotlin
class SoneSingleton private constructor(){

    companion object {

        @Volatile private var instance: SoneSingleton? = null

        fun getInstance(): SoneSingleton {
            return instance ?: synchronized(this) {
                instance ?: SoneSingleton().also { instance = it }
            }
        }
    }

```
