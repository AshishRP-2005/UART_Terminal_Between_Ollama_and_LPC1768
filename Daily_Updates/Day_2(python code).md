# Python Script
The python script will be running in the terminal of the PC

```
import serial
import ollama

PORT = "COM3"
BAUD = 9600

START_CHAR = '$'
END_CHAR = '#'

MASTER_PROMPT = (
    "Answer in less than 80 characters. "
    "Use simple English. "
    "No markdown. "
    "No lists. "
    "Give only the answer."
)

ser = serial.Serial(PORT, BAUD, timeout=1)

print("AI UART Bridge Started")

buffer = ""

while True:
    try:

        if ser.in_waiting:

            char = ser.read().decode(errors='ignore')

            if char == START_CHAR:
                buffer = ""

            elif char == END_CHAR:

                question = buffer.strip()

                print(f"\nRX: {question}")

                prompt = (
                    f"{MASTER_PROMPT}\n\n"
                    f"Question: {question}"
                )

                response = ollama.chat(
                    model="phi4-mini",
                    messages=[
                        {
                            "role": "user",
                            "content": prompt
                        }
                    ]
                )

                answer = response["message"]["content"]

                answer = answer.replace('\n', ' ')
                answer = answer.replace('$', '')
                answer = answer.replace('#', '')
                answer = answer[:80]

                tx_message = f"{START_CHAR}{answer}{END_CHAR}"

                print(f"TX: {tx_message}")

                ser.write(tx_message.encode())

                buffer = ""

            else:
                buffer += char

    except Exception as e:
        print("Error:", e)
```
