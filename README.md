# Voating_machine_2
ORG 0000H
MOV A, #38H     // use 2 lines and 5*7 
ACALL com    
MOV A, #0EH   //cursor blinking off 
ACALL com    
MOV A, #80H   // force cursor to first line 
ACALL com  
MOV A, #01H     // clr screen
ACALL com 


MOV R2, #00H
MOV R3, #00H
MOV R4, #00H
MOV R5, #00H


START: NOP
MOV R2, #00H
MOV R3, #00H
MOV R4, #00H
MOV R5, #00H
MOV A, #01H
ACALL com
MOV DPTR, #STR1  // Load the first message "Welcome!!"
REV1: MOV A, #00H   
MOVC A, @A+DPTR    
JZ FINISH1      
ACALL L_D    
INC DPTR    
SJMP REV1    
FINISH1: ACALL DELAY_ONE_SECOND  // Introduce a 1-second delay
MOV A, #01H     // clr screen
ACALL com   

DISPLAY_AGAIN:
MOV DPTR, #STR2  // Load the message "Press A to start"
REV2: MOV A, #00H   
MOVC A, @A+DPTR    
JZ FINISH2      
ACALL L_D    
INC DPTR    
SJMP REV2    
FINISH2: 

CHECK_BUTTON_A:
MOV A, P0      // Read Port 0 to check button A press
ANL A, #01H    // Check if button A is pressed (P0.2)
JZ CHECK_BUTTON_A   // If not pressed, keep checking
MOV A, #01H     // clr screen
ACALL com   

CLEAR_SCREEN:
MOV A, #01H     // clr screen
ACALL com  

LOOPING: NOP

DISPLAY_VOTE_OPTIONS:
MOV DPTR, #STR3  // Load 'P1 P2 P3 P4' in the first line
REV3: MOV A, #00H   
MOVC A, @A+DPTR    
JZ FINISH3      
ACALL L_D    
INC DPTR    
SJMP REV3    
FINISH3:

MOV A, #0C0H    // Move cursor to the second line, first column
ACALL com

MOV DPTR, #STR4  // Load '1 2 3 4' in the second line, first column
REV4: MOV A, #00H   
MOVC A, @A+DPTR    
JZ FINISH4      
ACALL L_D    
INC DPTR    
SJMP REV4    
FINISH4:

CHECK_BUTTON_1:
MOV A, P0      // Read Port 0 to check button A press
ANL A, #04H    // Check if button A is pressed (P0.2)
JZ CHECK_BUTTON_2   // If not pressed, keep checking
MOV A, #01H     // clr screen
INC R2
SJMP BUTTON_PRESSED


CHECK_BUTTON_2:
MOV A, P0      // Read Port 0 to check button A press
ANL A, #08H    // Check if button A is pressed (P0.2)
JZ CHECK_BUTTON_3   // If not pressed, keep checking
MOV A, #01H     // clr screen
INC R3
SJMP BUTTON_PRESSED

CHECK_BUTTON_3:
MOV A, P0      // Read Port 0 to check button A press
ANL A, #10H    // Check if button A is pressed (P0.2)
JZ CHECK_BUTTON_4   // If not pressed, keep checking
MOV A, #01H     // clr screen
INC R4
SJMP BUTTON_PRESSED


CHECK_BUTTON_4:
MOV A, P0      // Read Port 0 to check button A press
ANL A, #20H    // Check if button A is pressed (P0.2)
JZ CHECK_BUTTON_B   // If not pressed, keep checking
MOV A, #01H     // clr screen
INC R5
SJMP BUTTON_PRESSED

CHECK_BUTTON_B:
MOV A, P0      // Read Port 0 to check button A press
ANL A, #02H    // Check if button A is pressed (P0.2)
JZ CHECK_BUTTON_1   // If not pressed, keep checking
MOV A, #01H     // clr screen
ACALL com
SJMP ENDING

BUTTON_PRESSED:
    ; Handle button press here
    ; You can add the necessary code to increment vote counts, clear the screen, display "Thank you!", etc.
    ; For demonstration, let's just clear the screen and display "Thank you!"
    MOV A, #01H     // clr screen
    ACALL com
    
    MOV DPTR, #STR5  // Load "Thank you!" message
    REV5: MOV A, #00H   
    MOVC A, @A+DPTR    
    JZ FINISH5      
    ACALL L_D    
    INC DPTR    
    SJMP REV5    
    FINISH5: ACALL DEL_ROUTINE
    
    MOV A, #01H     // clr screen after displaying "Thank you!"
    ACALL com
	
	MOV DPTR, #STR7  // Load "Vote registered." message
    REV7: MOV A, #00H   
    MOVC A, @A+DPTR    
    JZ FINISH7      
    ACALL L_D    
    INC DPTR    
    SJMP REV7    
    FINISH7:
    
    MOV A, #01H     // clr screen after displaying "Thank you!"
    ACALL com

    LJMP LOOPING // After displaying "Thank you!", go back to looping

com: ACALL DEL_ROUTINE  
MOV P1, A   
CLR P2.1   
SETB P2.2
CLR P2.2
RET    

L_D: ACALL DEL_ROUTINE  
MOV P1, A      
SETB P2.1     
SETB P2.2     
CLR P2.2     
RET       

DELAY_ONE_SECOND:
    MOV R7, #250    // Load R7 with a value to create delay
DELAY_LOOP:
    DJNZ R7, DELAY_LOOP  // Decrement R7 until it reaches 0
    RET

DEL_ROUTINE: MOV R0, #0FFH  
L1: MOV R1, #0FFH   
L2: DJNZ R1, L2    
DJNZ R0, L1     
RET  


ENDING:MOV A, #01H     // clr screen
    ACALL com
    
    MOV DPTR, #STR6  // Load "Election Ended" message
    REV6: MOV A, #00H   
    MOVC A, @A+DPTR    
    JZ FINISH6      
    ACALL L_D    
    INC DPTR    
    SJMP REV6
	FINISH6:

MOV A, #01H     // clr screen
ACALL com 

MOV R1, #90H
MOV A, R2
MOV @R1, A
MOV R1, #91H
MOV A, R3
MOV @R1, A
MOV R1, #92H
MOV A, R4
MOV @R1, A
MOV R1, #93H
MOV A, R5
MOV @R1, A


MOV R4, #03H
MOV DPTR, #90H
MOVX A,@DPTR
MOV R2,A
AGAIN1: INC DPTR
MOVX A, @DPTR
MOV R3, A
CLR C
SUBB A, R1
JC OVER1
MOV R1, 02H
OVER1: DJNZ R4, AGAIN1
MOV DPTR, #95H
MOV A, R1
MOVX @DPTR, A


MOV A, 95H
CJNE A, 90H, CHECK2
HAHA: MOV DPTR, #STR10  // Load "Election Ended" message
    REV10: MOV A, #00H   
    MOVC A, @A+DPTR    
    JZ FINISH10      
    ACALL L_D    
    INC DPTR    
    SJMP REV10
	FINISH10: MOV R6, 95H
	LJMP CHECK_BUTTON_A1

MOV R1, #90H
MOV A, R2
MOV @R1, A
MOV R1, #91H
MOV A, R3
MOV @R1, A
MOV R1, #92H
MOV A, R4
MOV @R1, A
MOV R1, #93H
MOV A, R5
MOV @R1, A

CHECK2:
MOV R4, #03H
MOV DPTR, #90H
MOVX A,@DPTR
MOV R2,A
AGAIN2: INC DPTR
MOVX A, @DPTR
MOV R3, A
CLR C
SUBB A, R1
JC OVER2
MOV R1, 02H
OVER2: DJNZ R4, AGAIN2
MOV DPTR, #95H
MOV A, R1
MOVX @DPTR, A

	
MOV A, R6
CJNE A,91H, CHECK3
MOV DPTR, #STR11  // Load "Election Ended" message
    REV11: MOV A, #00H   
    MOVC A, @A+DPTR    
    JZ FINISH11      
    ACALL L_D    
    INC DPTR    
    SJMP REV11
	FINISH11: MOV R6, 95H
	SJMP CHECK_BUTTON_A1

MOV R1, #90H
MOV A, R2
MOV @R1, A
MOV R1, #91H
MOV A, R3
MOV @R1, A
MOV R1, #92H
MOV A, R4
MOV @R1, A
MOV R1, #93H
MOV A, R5
MOV @R1, A
	
CHECK3:
MOV R4, #03H
MOV DPTR, #90H
MOVX A,@DPTR
MOV R2,A
AGAIN3: INC DPTR
MOVX A, @DPTR
MOV R3, A
CLR C
SUBB A, R1
JC OVER3
MOV R1, 02H
OVER3: DJNZ R4, AGAIN3
MOV DPTR, #95H
MOV A, R1
MOVX @DPTR, A

CJNE A, 92H, CHECK4
MOV DPTR, #STR12  // Load "Election Ended" message
    REV12: MOV A, #00H   
    MOVC A, @A+DPTR    
    JZ FINISH12      
    ACALL L_D    
    INC DPTR    
    SJMP REV12
	FINISH12: MOV R6, 95H
	SJMP CHECK_BUTTON_A1



CHECK4:
MOV R4, #03H
MOV DPTR, #90H
MOVX A,@DPTR
MOV R2,A
AGAIN4: INC DPTR
MOVX A, @DPTR
MOV R3, A
CLR C
SUBB A, R1
JC OVER4
MOV R1, 02H
OVER4: DJNZ R4, AGAIN4
MOV DPTR, #95H
MOV A, R1
MOVX @DPTR, A

MOV A, R6
CJNE A, 93H, CHECK_BUTTON_A1
MOV DPTR, #STR13  // Load "Election Ended" message
    REV13: MOV A, #00H   
    MOVC A, @A+DPTR    
    JZ FINISH13      
    ACALL L_D    
    INC DPTR    
    SJMP REV13
	FINISH13: MOV R6, 95H
	SJMP CHECK_BUTTON_A1

CHECK_BUTTON_A1:
MOV A, P0      // Read Port 0 to check button A press
ANL A, #01H    // Check if button A is pressed (P0.2)
JZ CHECK_BUTTON_A1   // If not pressed, keep checking
MOV A, #01H     // clr screen
LJMP START   


STR1: DB 'Welcome!!',0  
STR2: DB 'Press A to start elections',0  
STR3: DB 'C1 C2 C3 C4',0
STR4: DB '1  2  3  4',0
STR5: DB 'Thank you!',0
STR7: DB 'Vote Registered.',0
STR6: DB 'Election Ended',0
	
STR10: DB 'C1 WINS',0
STR11: DB 'C2 WINS',0
STR12: DB 'C3 WINS',0
STR13: DB 'C4 WINS',0
	

    


END
