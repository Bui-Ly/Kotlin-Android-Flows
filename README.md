# Kotlin-Flows

## Documentation

* [Singleton instantiation](#singleton-instantiation)
* [Extensions](#extensions)
    - [Fragments](#fragments)
        - [Add fragment](#add-fragment)
        - [Repalce fragment](#repalce-fragment)
    - [Check all permission granted](#check-all-permission-granted)
 * [Request permission](#request-permission)
 * [Start activity for result](#start-activity-for-result)
 * 


## Singleton instantiation

```kotlin
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

## Extensions

Kotlin extensions

### Fragments

Fragments extension

#### Add fragment

```kotlin
fun AppCompatActivity.replaceFragment(
    fragment: Fragment,
    @IdRes containerViewId: Int,
    allowStateLoss: Boolean = false,
    @AnimRes enterAnimation: Int = R.anim.animation_enter,
    @AnimRes exitAnimation: Int = R.anim.animation_exit,
    @AnimRes popEnterAnimation: Int = R.anim.animation_pop_enter,
    @AnimRes popExitAnimation: Int = R.anim.animation_pop_exit
) {
    val ft = supportFragmentManager
        .beginTransaction()
        .setCustomAnimations(enterAnimation, exitAnimation, popEnterAnimation, popExitAnimation)
        .replace(containerViewId, fragment)
    if (!supportFragmentManager.isStateSaved) {
        ft.commit()
    } else if (allowStateLoss) {
        ft.commitAllowingStateLoss()
    }
}
```

#### Repalce fragment

```kotlin
fun AppCompatActivity.addFragment(
    fragment: Fragment,
    @IdRes containerViewId: Int,
    allowStateLoss: Boolean = false,
    @AnimRes enterAnimation: Int = R.anim.animation_enter,
    @AnimRes exitAnimation: Int = R.anim.animation_exit,
    @AnimRes popEnterAnimation: Int = R.anim.animation_pop_enter,
    @AnimRes popExitAnimation: Int = R.anim.animation_pop_exit
) {
    val ft = supportFragmentManager
        .beginTransaction()
        .setCustomAnimations(enterAnimation, exitAnimation, popEnterAnimation, popExitAnimation)
        .addToBackStack(null)
        .add(containerViewId, fragment)
    if (!supportFragmentManager.isStateSaved) {
        ft.commit()
    } else if (allowStateLoss) {
        ft.commitAllowingStateLoss()
    }
}
```

### Check all permission granted

return true if all permission granted

```kotlin
fun AppCompatActivity.hasPermissions(permissions: Array<String>): Boolean =
    permissions.all {
        ActivityCompat.checkSelfPermission(this, it) == PackageManager.PERMISSION_GRANTED
    }
```

## Request permission

step 1: Permission need

```kotlin
var permissions = arrayOf(
        Manifest.permission.CAMERA
    )
```

step 2: request permission

```kotlin
private val permissionRequest =
        registerForActivityResult(ActivityResultContracts.RequestMultiplePermissions()) { permissions ->
            val granted = permissions.entries.all {
                it.value == true
            }
            if (granted) {
                // do somethings
            }
        }
```

step 3: launch

```kotlin
if (mActivity.hasPermissions(permissions)) {
        // has permission
    } else {
        permissionRequest.launch(permissions)
    }
```

## Start activity for result

step 1: init

```kotlin
private val activityLauncher =
        registerForActivityResult(ActivityResultContracts.StartActivityForResult()) { result ->
            val resultCode = result.resultCode
            val data = result.data
            // do somethings
        }
```

step 2: launch
```kotlin
val intent = Intent(context, SomeActivity::class.java)
activityLauncher.launch(intent);
```


