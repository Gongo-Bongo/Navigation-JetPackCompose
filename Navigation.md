In Jetpack Compose, navigation between screens can be managed using the **Jetpack Navigation Component**, which now has built-in support for Compose. Here’s a breakdown of how to set up and use navigation in a Jetpack Compose app with examples.

### 1. Setting Up Navigation Dependencies
First, add the necessary dependencies in your `build.gradle` file:

```gradle
dependencies {
    implementation "androidx.navigation:navigation-compose:2.5.3"
}
```

### 2. Basic Structure of Navigation in Jetpack Compose
Navigation in Jetpack Compose typically involves setting up a **NavHost** and **NavController**. A `NavHost` is a container that hosts all your app's screens and manages the navigation states.

### 3. Creating Screens (Composable Functions)
Define your screens as composable functions. Each screen will be represented by a unique **route**.

```kotlin
@Composable
fun HomeScreen(navController: NavController) {
    Column(
        modifier = Modifier.fillMaxSize(),
        horizontalAlignment = Alignment.CenterHorizontally,
        verticalArrangement = Arrangement.Center
    ) {
        Text("Home Screen")
        Button(onClick = { navController.navigate("detail_screen") }) {
            Text("Go to Detail Screen")
        }
    }
}

@Composable
fun DetailScreen(navController: NavController) {
    Column(
        modifier = Modifier.fillMaxSize(),
        horizontalAlignment = Alignment.CenterHorizontally,
        verticalArrangement = Arrangement.Center
    ) {
        Text("Detail Screen")
        Button(onClick = { navController.navigate("home_screen") }) {
            Text("Back to Home Screen")
        }
    }
}
```

### 4. Setting Up NavController and NavHost
The **NavController** controls navigation actions, while the **NavHost** defines and manages the composable destinations and the routes between them.

Here’s how to set up the `NavHost`:

```kotlin
@Composable
fun Navigation() {
    val navController = rememberNavController()
    NavHost(navController = navController, startDestination = "home_screen") {
        composable("home_screen") { HomeScreen(navController) }
        composable("detail_screen") { DetailScreen(navController) }
    }
}
```

- **`startDestination`**: This is the first screen shown in the app.
- **`composable` function**: This function takes a route as an argument and maps it to a composable screen.

### 5. Navigating Between Screens
Navigation happens using the **navigate()** function of `NavController`. For instance, in the `HomeScreen` function, we used:

```kotlin
Button(onClick = { navController.navigate("detail_screen") })
```

This navigates from the `HomeScreen` to the `DetailScreen` using the specified route, `"detail_screen"`.

### 6. Passing Data Between Screens
To pass data, you can define arguments in your route. For example, let’s add a parameter to the `DetailScreen`:

1. Update the route to accept a parameter in `NavHost`:

```kotlin
NavHost(navController = navController, startDestination = "home_screen") {
    composable("home_screen") { HomeScreen(navController) }
    composable("detail_screen/{itemId}") { backStackEntry ->
        DetailScreen(navController, itemId = backStackEntry.arguments?.getString("itemId"))
    }
}
```

2. Modify the `HomeScreen` to pass data when navigating:

```kotlin
Button(onClick = { navController.navigate("detail_screen/${itemId}") }) {
    Text("Go to Detail Screen with Item ID")
}
```

3. Update `DetailScreen` to accept the parameter:

```kotlin
@Composable
fun DetailScreen(navController: NavController, itemId: String?) {
    Column(
        modifier = Modifier.fillMaxSize(),
        horizontalAlignment = Alignment.CenterHorizontally,
        verticalArrangement = Arrangement.Center
    ) {
        Text("Detail Screen for item ID: $itemId")
        Button(onClick = { navController.popBackStack() }) {
            Text("Back to Home Screen")
        }
    }
}
```

### 7. Handling Back Navigation
To navigate back, use `popBackStack()`:

```kotlin
navController.popBackStack()
```

This will remove the current screen from the stack and take the user back to the previous screen.

### Complete Example
```kotlin
@Composable
fun MyApp() {
    Navigation()
}

@Composable
fun Navigation() {
    val navController = rememberNavController()
    NavHost(navController = navController, startDestination = "home_screen") {
        composable("home_screen") { HomeScreen(navController) }
        composable("detail_screen/{itemId}") { backStackEntry ->
            DetailScreen(navController, itemId = backStackEntry.arguments?.getString("itemId"))
        }
    }
}

@Composable
fun HomeScreen(navController: NavController) {
    val itemId = "123"
    Column(
        modifier = Modifier.fillMaxSize(),
        horizontalAlignment = Alignment.CenterHorizontally,
        verticalArrangement = Arrangement.Center
    ) {
        Text("Home Screen")
        Button(onClick = { navController.navigate("detail_screen/$itemId") }) {
            Text("Go to Detail Screen")
        }
    }
}

@Composable
fun DetailScreen(navController: NavController, itemId: String?) {
    Column(
        modifier = Modifier.fillMaxSize(),
        horizontalAlignment = Alignment.CenterHorizontally,
        verticalArrangement = Arrangement.Center
    ) {
        Text("Detail Screen for item ID: $itemId")
        Button(onClick = { navController.popBackStack() }) {
            Text("Back to Home Screen")
        }
    }
}
```

### Key Points
- **NavController**: Manages app navigation.
- **NavHost**: Defines navigation routes and links them to composable functions.
- **navigate()**: Initiates screen transitions.
- **popBackStack()**: Returns to the previous screen.

This is a basic setup that you can further extend with custom animations, deep links, and nested navigation.
