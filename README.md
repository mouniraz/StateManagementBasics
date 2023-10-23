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
) { LazyColumn(
        modifier = modifier
    ) {
        items(items = list,
            key = { task -> task.id })
        { task ->
            WellnessTaskItem(
                task = task, )
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
## Use A TakViewModel to maintain state
in the place of declaring a state in the WellnessScreen, we will declare a ViewModel to declare the state and operations of state 
1. declare a ViewModelTak extendink a ViewModel Class
```kotlin
   class WellnessViewModel: ViewModel()
{
    private val _statelist= getWellnessTasks().toMutableStateList();
    val statelist:List<WellnessTask>
        get() = _statelist
    fun onCheckChange(item: WellnessTask)
    {
        _statelist.toList().find { it.id==item.id }?.let{
            it.checkedState.value=!it.checkedState.value
        }
    }
   }
   private fun getWellnessTasks() = List(30) { i -> WellnessTask(i, "Task # $i") }
   ```
2. Declare and instantiate a ViewModel in WellnessScreen
```kotlin
 val wellnessViewModel=WellnessViewModel()
    Column(modifier = modifier) { WellnessTasksList(
            list = wellnessViewModel.statelist,
            oncheckChange =  {wellnastask->wellnessViewModel.onCheckChange(wellnastask)}       )
    }
```
3. redefine WellnessTasksList
   ```kotlin
   fun WellnessTasksList(
    list: List<WellnessTask>,
    oncheckChange:(WellnessTask)->Unit,
    modifier: Modifier = Modifier

) {    LazyColumn(
        modifier = modifier
    ) {        items(items = list,
            key = { task -> task.id })
        { task ->
            WellnessTaskItem(
                task = task,
                onCheckChange = { oncheckChange(task) },
            )
   ```

5. redefine WellnessTaskItem
```kotlin
@Composable
fun WellnessTaskItem(
    task:WellnessTask,
    onCheckChange:(WellnessTask)->Unit,
    modifier: Modifier = Modifier
)
{
..........
   Checkbox(
            checked = task.checkedState.value,
            onCheckedChange = {onCheckChange(task)}
        )
   
}
````
