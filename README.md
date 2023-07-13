# Live Templates
## This The following code snippet refers to code templates that we have in Android Studio 


## View model template

```Kotlin  #if (${PACKAGE_NAME} && ${PACKAGE_NAME} != "")package ${PACKAGE_NAME}#end

import androidx.lifecycle.ViewModel
import androidx.lifecycle.viewModelScope
import com.sa.common.status.ViewState
import dagger.hilt.android.lifecycle.HiltViewModel
import kotlinx.coroutines.flow.MutableStateFlow
import kotlinx.coroutines.flow.StateFlow
import kotlinx.coroutines.launch
import javax.inject.Inject

@HiltViewModel
class ${NAME} @Inject constructor(
    private val ${useCase}: ${useCase}
) : ViewModel() {

    private var _emit: MutableStateFlow<ViewState<${Response}>?> =
        MutableStateFlow(null)
    val emit: StateFlow<ViewState<${Response}>?> = _emit


    fun request${NAME}() {
        _emit.value = ViewState.Loading

        viewModelScope.launch(Dispatchers.IO) {
            when (val response = ${useCase}.invoke()) {
                is ViewState.Success -> _emit.value = response
                is ViewState.Error -> _emit.value = response
                else -> ViewState.Error(Exception("some error"))
            }
        }
    }
}
```
## Adapter template
```Kotlin 
#if (${PACKAGE_NAME} && ${PACKAGE_NAME} != "")package ${PACKAGE_NAME}#end

import android.content.Context
import android.view.LayoutInflater
import android.view.View
import android.view.ViewGroup
import androidx.recyclerview.widget.DiffUtil
import androidx.recyclerview.widget.RecyclerView
import androidx.recyclerview.widget.ListAdapter

class ${NAME} : RecyclerView.Adapter<${NAME}.${Model}ViewHolder >()  {
     
      private lateinit var _itemClickListener: ItemClickListener
      private var myDataset: List<${Model}> = ArrayList()
          
    fun submitData(dataset: List<${Model}>) {
        myDataset = dataset
     }
     
     fun subscribeListener(itemClickListener: ItemClickListener) {
        _itemClickListener = itemClickListener
    }
    
    override fun getItemCount() = myDataset.size
          
    override fun onCreateViewHolder(parent: ViewGroup, viewType: Int): ${Model}ViewHolder {
        val context = parent.context
        val binding = ${row}.inflate(
            LayoutInflater.from(context),
            parent,
            false
        )
        return ${Model}ViewHolder(binding, _itemClickListener, context)
    }
    
    override fun onBindViewHolder(holder: ${Model}ViewHolder, position: Int) {
        holder.bind(myDataset[position], position)
    }
    
    class ${Model}ViewHolder constructor(
        private val binding: ${row},
        private val _itemClickListener: ItemClickListener,
        private val context: Context
    
    ) : RecyclerView.ViewHolder(binding.root){
         fun bind(item: ${Model}, position: Int) {
         //TODO: Bind your views here     
        }
    }
    
    class ${Model}DiffCallback() : DiffUtil.ItemCallback<${Model}>() {
    override fun areItemsTheSame(oldItem: ${Model}, newItem: ${Model}): Boolean {
    //the one in your model
        return oldItem.id == newItem.id
    }
    
    override fun areContentsTheSame(oldItem: ${Model}, newItem: ${Model}): Boolean {
        return oldItem == newItem
    }
}
    interface ItemClickListener {
        fun onItemClick(model: ${Model})
    }
}

```
## Live template for init Observe 
```Kotlin
private fun observe$NAME$() {
    lifecycleScope.launch {
            viewModel.$MUTABLE$.collect { response ->
                when (response) {
                    is ViewState.Loading -> {
                        binding.progressBar.root.visible()
                    }
                    is ViewState.Success -> {
                        binding.progressBar.root.gone()
                        on$NAME$Response(response.result)
                    }
                    is ViewState.Error -> {
                        binding.progressBar.root.gone()
                        handleErrorMessage(response){}
                    }
                    else -> {}
                }
            }
        }
     }
```
# Lie template for mutableStateFlow
```Kotlin
    private var _$NAME$: MutableStateFlow<ViewState<$RESPONSE$>?> =
        MutableStateFlow(null)
    val $NAME$: StateFlow<ViewState<$RESPONSE$>?> = _$NAME$


    fun request$NAME$() {
        _$NAME$.value = ViewState.Loading

        viewModelScope.launch {
            when (val response = $USECASE$.invoke()) {
                is ViewState.Success -> _$NAME$.value = response
                is ViewState.Error -> _$NAME$.value = response
                else -> ViewState.Error(Exception("some error"))
            }
        }
    }
```
