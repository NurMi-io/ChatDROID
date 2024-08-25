`MainActivity.kt`는 안드로이드 앱에서 가장 중심이 되는 Kotlin 파일로, 사용자 인터페이스와 기능을 초기화하고 관리하는 역할을 합니다. ChatGOV 앱을 위한 `MainActivity.kt` 파일을 구성하는 방법을 아래에 예시 코드와 함께 설명하겠습니다. 이 예시는 Material Design 3 지침을 따르며, 앱 바, 채팅 메시지, 텍스트 입력 기능을 포함하고, 심심이 워크숍 API와의 연동을 고려합니다.

### MainActivity.kt 예시

```kotlin
import android.os.Bundle
import androidx.appcompat.app.AppCompatActivity
import androidx.recyclerview.widget.LinearLayoutManager
import androidx.recyclerview.widget.RecyclerView
import com.google.android.material.textfield.TextInputEditText
import com.google.android.material.button.MaterialButton
import com.google.android.material.appbar.MaterialToolbar
import kotlinx.coroutines.*

class MainActivity : AppCompatActivity() {
    private lateinit var chatAdapter: ChatAdapter
    private lateinit var recyclerView: RecyclerView
    private lateinit var inputField: TextInputEditText
    private lateinit var sendButton: MaterialButton
    private lateinit var toolbar: MaterialToolbar

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        toolbar = findViewById(R.id.toolbar)
        setSupportActionBar(toolbar)

        recyclerView = findViewById(R.id.chat_recycler_view)
        recyclerView.layoutManager = LinearLayoutManager(this)

        chatAdapter = ChatAdapter()
        recyclerView.adapter = chatAdapter

        inputField = findViewById(R.id.input_text)
        sendButton = findViewById(R.id.send_button)

        sendButton.setOnClickListener {
            val message = inputField.text.toString()
            if (message.isNotEmpty()) {
                sendMessage(message)
                inputField.text?.clear()
            }
        }
    }

    private fun sendMessage(message: String) {
        // Display the user's message in the UI
        chatAdapter.addMessage(ChatMessage(message, MessageType.USER))

        // Simulate a delay for API response
        GlobalScope.launch(Dispatchers.Main) {
            delay(1000)
            val response = callSimSimiApi(message)
            chatAdapter.addMessage(ChatMessage(response, MessageType.AI))
            recyclerView.scrollToPosition(chatAdapter.itemCount - 1)
        }
    }

    private fun callSimSimiApi(message: String): String {
        // This function should implement the actual API call to the SimSimi Workshop API
        // For now, let's return a dummy response
        return "This is a response to '$message'"
    }
}

data class ChatMessage(val message: String, val type: MessageType)
enum class MessageType {
    USER, AI
}
```

### 설명
1. **초기 설정 및 레이아웃 연결**: `setContentView`를 사용하여 XML 레이아웃 파일을 액티비티에 연결합니다.
2. **앱 바 설정**: `MaterialToolbar`를 사용하여 앱 바를 설정합니다.
3. **리사이클러 뷰 및 어댑터**: 채팅 메시지를 보여줄 `RecyclerView`와 해당 어댑터를 초기화합니다.
4. **텍스트 입력 및 전송 버튼**: 사용자 입력을 받는 `TextInputEditText`와 메시지를 보내는 `MaterialButton`을 설정합니다.
5. **메시지 전송 함수**: 사용자가 메시지를 입력하면 화면에 표시하고, API를 호출하여 AI의 응답을 받아 표시합니다.

이 코드는 기본적인 채팅 인터페이스와 API 연동을 설명하기 위한 것이며, 실제 애플리케이션에서는 API 키 관리, 에러 처리, 네트워크 상태 확인 등 보다 세부적인 기능이 추가될 필요가 있습니다.
