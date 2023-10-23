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
5. scroll your list and say what´is the problem with the checkboxs
6. To correct this problem, we will host the checkboxes states in a mutableStateList of WellnessTasks
to do this, we will
a. modify the declaration of WellnessTask
```kotlin
data  class WellnessTask(
    val id: Int,
    val label: String,
    var checkedState: MutableState<Boolean> =  mutableStateOf(false)
)
```
b. declare the mutableStateList in the WillnessScreen Like this
```kotlin
    var statelist= getWellnessTasks().toMutableStateList();
 Column(modifier = modifier) {

        WellnessTasksList(
            list = statelist.toList(),
        )

```
c. do Modification in TaskItem 
```kotlin
 Checkbox(
            checked = task.checkedState.value,
            onCheckedChange = {task.checkedState.value=!task.checkedState.value}
        )
```
