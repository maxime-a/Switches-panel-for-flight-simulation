# Switches panel for flight simulation
Proof of concept to be developped, master, avionics, lights and landing gear switches with rgb led for landing gear position.

I use an arduino undo to communicate via serial com port to a software "Link2fs" which allow me to extract data from microsoft flight simulator and send inputs to the simulator. 


## Schematics

## Code

```C++
int CodeIn;
String gearSimple;

void setup() {
  pinMode(2, OUTPUT); // gear nose LED
  pinMode(5, OUTPUT); // gear nose in transition LED


  pinMode(6, INPUT_PULLUP);
  pinMode(7, INPUT_PULLUP);
  pinMode(8, INPUT_PULLUP);
  pinMode(9, INPUT_PULLUP);
  pinMode(10, INPUT_PULLUP);
  pinMode(11, INPUT_PULLUP);

  Serial.begin(115200);
}

void loop() {

 if (digitalRead(10) == LOW){Serial.println ("C01");} //sets gear handle up
 if (digitalRead(11) == LOW){Serial.println ("C02");} //sets gear handle down
        
        if (digitalRead(7) == HIGH){Serial.println ("C441");} //Taxi light on
        else {Serial.println ("C440");}//Taxi light off
        
        if (digitalRead(8) == HIGH){Serial.println ("C431");}//Landing light on
        else {Serial.println ("C430");}//Landing light off
        
        if (digitalRead(9) == HIGH){Serial.println ("A431");}//Avionics on
        else {Serial.println ("A430");}//Landing light off
        
        if (digitalRead(6) == LOW){Serial.println ("E18");}//Master on
        else {Serial.println ("E17");}//Master off
  
 if (Serial.available())
 {//checks the serial read buffer
    CodeIn = getChar();// reads one charactor via it's own routine (char getChar)
    if (CodeIn == '?') {QUESTION();}// The first identifier is "?", goto QUESTION void
 }

}

void QUESTION(){    // The first identifier was "?"
CodeIn = getChar(); // Get the second identifier
  switch(CodeIn) {// Now lets find what to do with it
    case 'Y': // found the second identifier (the "Gear simple")
       gearSimple = "";
      gearSimple += getChar();// get first charactor (Nose gear)
      if (gearSimple == "2"){digitalWrite(2, HIGH);}else{digitalWrite(2, LOW);}
      if (gearSimple == "1"){digitalWrite(5, HIGH);}else{digitalWrite(5, LOW);}
    break;
     }
}// end of question void

char getChar()// Get a character from the serial buffer
{
  while(Serial.available() == 0);// wait for data
  return((char)Serial.read());// Thanks Doug
}// end of getchar void. 

```
