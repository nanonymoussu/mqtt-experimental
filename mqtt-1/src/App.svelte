<script lang="ts">
  import mqtt from 'mqtt'

  const brokerUrl = 'ws://test.mosquitto.org:8080'
  const topic = 'chat/topic'

  const SENDER_ID = 'Website 1'

  let message = ''
  let messages: { sender: string; text: string }[] = []

  let client = mqtt.connect(brokerUrl)
  client.on('connect', () => {
    console.log('Website 1 connected to MQTT broker')
    client.subscribe(topic, (err) => {
      if (!err) console.log(`Subscribed to ${topic}`)
    })
  })

  client.on('message', (receivedTopic, payload) => {
    if (receivedTopic === topic) {
      const { sender, text } = JSON.parse(payload.toString())
      if (sender !== SENDER_ID) {
        messages = [...messages, { sender: 'Other', text }]
      }
    }
  })

  function sendMessage() {
    if (message.trim()) {
      const payload = JSON.stringify({ sender: SENDER_ID, text: message })
      client.publish(topic, payload)
      messages = [...messages, { sender: SENDER_ID, text: message }]
      message = ''
    }
  }
</script>

<main>
  <h1>MQTT - 1</h1>
  <div class="chat-container">
    <div class="messages">
      {#each messages as msg}
        <div class="message {msg.sender === SENDER_ID ? 'you' : 'other'}">
          {msg.text}
        </div>
      {/each}
    </div>
    <div class="input-container">
      <input
        type="text"
        bind:value={message}
        placeholder="Type a message..."
        on:keydown={(e) => e.key === 'Enter' && sendMessage()}
      />
      <button on:click={sendMessage}>Send</button>
    </div>
  </div>
</main>

<style>
  main {
    font-family: Arial, sans-serif;
    width: 321px;
    margin: 0 auto;
    padding: 15px;
    border: 1px solid #ccc;
    border-radius: 10px;
    background-color: #f9f9f9;
  }

  h1 {
    text-align: center;
    color: #333;
  }

  .chat-container {
    display: flex;
    flex-direction: column;
    gap: 10px;
  }

  .messages {
    height: 123px;
    overflow-y: auto;
    border: 1px solid #ccc;
    border-radius: 5px;
    padding: 10px;
    background-color: #fff;
    color: #333;
    display: flex;
    flex-direction: column;
    gap: 10px;
  }

  .message {
    max-width: 70%;
    padding: 10px;
    border-radius: 10px;
    word-wrap: break-word;
  }

  /* YOU */
  .message.you {
    background-color: #007bff;
    color: white;
    align-self: flex-end;
    margin-left: auto;
  }

  /* OTHER */
  .message.other {
    background-color: #e9ecef;
    color: #333;
    align-self: flex-start;
    margin-right: auto;
  }

  .input-container {
    display: flex;
    gap: 10px;
  }

  input {
    flex: 1;
    padding: 10px;
    border: 1px solid #ccc;
    border-radius: 5px;
  }

  button {
    padding: 10px 20px;
    border: none;
    border-radius: 5px;
    background-color: #007bff;
    color: white;
    cursor: pointer;
  }

  button:hover {
    background-color: #0056b3;
  }
</style>
