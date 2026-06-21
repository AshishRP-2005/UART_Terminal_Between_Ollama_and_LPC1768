# Day 1
##  Test codes I have written

## Main code and global variables
```
void main (void)
{

//Global Variables

char Message[128] = "";
char visited;

char currentKey = 0;
char currentchar = 0 ;

unsigned int count = 0;
unsigned int cursor = 0;

char *T9[] =
{
    "",
    "",
    "ABC",
    "DEF",
    "GHI",
    "JKL",
    "MNO",
    "PQRS",
    "TUV",
    "WXYZ"
};
}
// End of Global Variables
{
char key;

Sys_Init();
GLCD_Clear(White);

Timer0_Init();

while(1)
{
key = Keypad_GetKey();

if (key >= '2' && key <='9')
{
Processing(key);

UpdateDisplay();

}

if (key == '*')
{
Backspace();
UpdateDisplay();
}

if (key == '0')
{
CommitCharacter();

message[cursor++] = ' ';
message[cursor] = '\0';

UpdateDisplay();
}

if (key == '#')
{
CommitCharacter();

UpdateDisplay();

// Uart0_TXstring
}

if(Timer0_Expired())
{
CommitCharacter();

UpdateDisplay();
}
}
}
```
## Functions
```
//Functions 

void Commit(void)
{
if(currentChar)
{
	message[cursor++] = currentChar;
	message[cursor] = '\0';
}
currentChar = 0;
count = 0;
currentKey = 0;
}

void Processing(char key)
{
 char *temp;
 int index;
 int len;

if (key == currentKey)
{
 count++
}
else 
{
currentKey = key;
count = 0;
}

temp = T9[key - '0'];
len = strlen(temp);
index = count % len;
currentChar = temp[index];
Timer0_Reset();
}

void Timer0_Init(void)
{
LPC_SC->PCONP |= (1<<1);

LPC_TIM0->TCR = 0x02;

LPC_TIM0->PR = 24999;

LPC_TIM0->MR0 = 2000;

LPC_TIM0->MCR = 0x03;

LPC_TIM0->TCR = 0x01;
}

void Timer0_Reset(void)
{
LPC_TIM0->TCR = 0x02;

LPC_TIM0->TCR = 0x01;
}

unsigned char Timer0_Expired(void)
{
if(LPC_TIM0->IR & 1)
{

LPC_TIM0->IR = 1;
return 1;
}

return 0;
}

void UpdateDisplay(void)
{
char display[130];
sprint(display,"%s%c",M\message,currentChar);

GLCD_Clear(White);

GLCD_DisplayString(5,0,1,(unsigned char *)display);

}

void Backspace(void)
{
if (cursor >0)
{ 
cursor--;
message[cursor] = '\0';
}
}
```



