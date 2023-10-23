# StateManagementBasics

## Create User Interface using Compose 
1. Create a data class named TaskWellness in your data package
   ```kotlin
   data  class WellnessTask(
    val id: Int,
    val label: String,
    val initialChecked: Boolean = false)
   ```
2. To create a Lazy column create first TaskWellnessItem (fill the blanks with correct proposition)
```Kotlin
@Composable
fun WellnessTaskItem(
    task:WellnessTask,
    modifier: Modifier = Modifier
) {
    var checkedState = remember { mutableStateOf(...........) }

    Row(
        modifier = modifier, verticalAlignment = Alignment.CenterVertically
    ) {
        Text(
            modifier = Modifier
                .weight(1f)
                .padding(start = 16.dp),
            text = task.label
        )
        Checkbox(
            checked = ........................,
            onCheckedChange = {.....................}
        )
        IconButton(onClick = {  }) {
            Icon(Icons.Filled.Close, contentDescription = "Close")
        }
    }
}
```
3. Create a list of WellnessItems using LazyColumn
```kotlin
@Composable
fun WellnessTasksList(
    list: List<WellnessTask>,
    modifier: Modifier = Modifier
) {
    LazyColumn(
        modifier = modifier
    ) {
        items(items = list,
            key = { task -> task.id })
        { task ->
            WellnessTaskItem(
                task = task,

            )
        }
    }
}
```
4. Create a WellnessScreen and Test your MainActivity with a List of WellnessTasks 

```kotlin
private fun getWellnessTasks() = List(30) { i -> WellnessTask(i, "Task # $i") }

@Composable
fun WellnessScreen(
    modifier: Modifier = Modifier,
) {
    Column(modifier = modifier) {

        WellnessTasksList(
            list = getWellnessTasks(),
        )
    }
}
```
  
