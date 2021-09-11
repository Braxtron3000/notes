# What is mvvm?

Model, View, ViewModel

component | purpose | 
--|--
Model | for retrieving data
View | shows the UI
ViewModel | presentation logic, prepares the data in a way views can easily retrieve it

this image may help explain
![image](../../notes_images/android/separated_presentation.jpg)


Android provides the [ViewModel](https://developer.android.com/topic/libraries/architecture/viewmodel) class.

it also provides a [binding library](https://developer.android.com/topic/libraries/data-binding) (i think to help the viewmodel to retrieve data but i haven't read that far yet🥴)

it helps...
* the UI data to survive configuration changes like screen rotations.
* simplify a complex program 

