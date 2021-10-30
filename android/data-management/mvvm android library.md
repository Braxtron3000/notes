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

it also provides a [binding library](https://developer.android.com/topic/libraries/data-binding)

it helps...
* the UI data to survive configuration changes like screen rotations.
* simplify a complex program 

# how to implement mvvm according to the book
there is also an activity project template when creating a new project that has a modelview+fragment or something like that along those lines.


## 1. getting started

1. gradle depedency in the module section
```
android {

    dataBinding{
        enabled=true
    }

```
2. create your layout.xml file(s)

* add a layout view as your root view
* add a data element to it
```
<layout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools">

    <data>

        <variable
            name="myViewModel"
            type="com.example.viewmodeldemo.ui.main.MainViewModel" />
    </data>

    <androidx.constraintlayout.widget.ConstraintLayout
        android:id="@+id/main"
```

## 2. viewmodel class

1. create a class that extends the viewmodel

```
public class MainViewModel extends ViewModel {
```
2. then go ahead and add the fields you'd like to have liveData.

as someone in r/androiddevelopers said "R.findViewById is dead Livedata has taken it's place" or something like that

```
    public MutableLiveData<String> dollarValue = new MutableLiveData<>();
    public MutableLiveData<Float> result = new MutableLiveData<>();
```

3. also add the datamanipulation methods you'd like to happen whenever data is changed

```
public void convertValue() {
        if ((dollarValue.getValue() != null) &&
                (!dollarValue.getValue().equals(""))) {
            result.setValue(Float.valueOf(dollarValue.getValue())
                    * usd_to_eu_rate);
        }
        else result.setValue(0F);
    }
```

## 3. add it to your fragment/activity class

1. import some stuff it might be automatically added by android studio but i'll list it. better safe than spending a long time looking for the package.

```
import androidx.databinding.DataBindingUtil;
import com.example.viewmodeldemo.databinding.MainFragmentBinding;
import static com.example.viewmodeldemo.BR.myViewModel;

import com.example.viewmodeldemo.R;
```

2. add a MainViewModel private instance variable and a youclassbinding object intance variable to the class

```
private MainViewModel mViewModel;

public MainFragmentBinding binding;
```

3. in the oncreateview.....
change the inflate method to this 

```
 binding = DataBindingUtil.inflate(inflater,R.layout.main_fragment,container,false);
 ```
and then
```
return binding.getRoot();
```

4. in the onActivityCreated.....

```
mViewModel = new ViewModelProvider(this).get(MainViewModel.class);
        binding.setVariable(myViewModel,mViewModel);
```

5. finally in the layout.xml file add all the stuff that you want changed

```
<TextView
            android:id="@+id/resultText"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
```

this line right here to text basically use safeUnbox with the myViewModel. instance variable
```
android:text='@{safeUnbox(myViewModel.result) == 0.0 ? "Enter value" : String.valueOf(safeUnbox(myViewModel.result)) + "euros"}'
```
```
            app:layout_constraintBottom_toBottomOf="parent"
            app:layout_constraintEnd_toEndOf="parent"
            app:layout_constraintStart_toStartOf="parent"
            app:layout_constraintTop_toTopOf="parent" />
```


here's a couple more examples. look at the text and the onclick

```
<EditText
            android:id="@+id/dollarText"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:layout_marginBottom="90dp"
            android:autofillHints=""
            android:ems="10"
            android:text="@={myViewModel.dollarValue}"
            android:inputType="numberDecimal"
            app:layout_constraintBottom_toTopOf="@+id/resultText"
            app:layout_constraintEnd_toEndOf="parent"
            app:layout_constraintStart_toStartOf="parent"
            tools:ignore="SpeakableTextPresentCheck" />
```



```
   <Button
            android:id="@+id/convertButton"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:layout_marginTop="81dp"
            android:text="@string/convert"
            android:onClick="@{() -> myViewModel.convertValue()}"
            app:layout_constraintEnd_toEndOf="parent"
            app:layout_constraintStart_toStartOf="parent"
            app:layout_constraintTop_toBottomOf="@+id/resultText" />
```