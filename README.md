# Code-Conversion-Using-8085
## Aim:
## To write 8085 microprocessor programs for converting:
1.	Hexadecimal to ASCII
2.	ASCII to Hexadecimal
## Apparatus Required:
•	Laptop with an internet connection
## Program 1: Hexadecimal to ASCII Conversion
ORG 00H
; Input : Port 00H (Hexadecimal number)
; Output: Port 01H (ASCII of upper nibble)
;         Port 02H (ASCII of lower nibble)

MVI A, 00H
IN 00H        ; Read hexadecimal number
MOV B, A      ; Save a copy of original number

ANI 0F0H      ; Mask lower nibble
RRC
RRC
RRC
RRC           ; Move upper nibble to lower position
CPI 0AH       ; Compare with 0AH
JC ADD30U     ; If less than 0AH, jump
ADI 37H       ; Add 37H for A-F
JMP STORE1

ADD30U: ADI 30H ; Add 30H for 0-9

STORE1: OUT 01H ; Send upper nibble ASCII to port 01H

; Process lower nibble
MOV A, B       ; Get original number
ANI 0FH        ; Mask upper nibble
CPI 0AH        ; Compare with 0AH
JC ADD30L      ; If less, jump
ADI 37H        ; Add 37H for A-F
JMP STORE2

ADD30L: ADI 30H ; Add 30H for 0-9

STORE2: OUT 02H ; Send lower nibble ASCII to port 02H

HLT             ; Stop program
END
## Algorithm:
1.	Load the hexadecimal number from memory location 4200H.
2.	Mask the upper nibble and check if it is less than 10H.
3.	If it is less than 10H, add 30H to convert it to ASCII.
4.	If it is greater than 10H, add 37H to convert it to ASCII.
5.	Repeat the process for the lower nibble.
6.	Store the ASCII equivalent in memory location 4300H and 4301H.
## Output:
<img width="1853" height="823" alt="image" src="https://github.com/user-attachments/assets/e3b1754c-6571-4567-af1b-57702e50c7e6" />
Input Ports (numbers are read from these ports):
 00H → Input hexadecimal number
Output Ports (results are displayed on these ports):
 01H → ASCII of upper nibble
 02H → ASCII of lower nibble
•	The ASCII equivalent of the hexadecimal number will be stored in 4300H (upper nibble) and 4301H (lower nibble).

## Program 2: ASCII to Hexadecimal Conversion
ORG 00H
; Input : Port 00H → ASCII of upper nibble
;          Port 01H → ASCII of lower nibble
; Output: Port 02H → Combined hexadecimal number

; --- Convert upper ASCII nibble to HEX ---
IN 00H          ; Read upper ASCII digit
CPI 3AH         ; Compare with '9'+1 (3AH)
JC SUB30U       ; If less than 3AH, it’s 0–9
SUI 37H         ; Else subtract 37H for A–F
JMP STOREU

SUB30U: SUI 30H ; Subtract 30H for 0–9 conversion
STOREU: MOV C, A ; Store upper nibble in C

; --- Convert lower ASCII nibble to HEX ---
IN 01H          ; Read lower ASCII digit
CPI 3AH         ; Compare with '9'+1 (3AH)
JC SUB30L       ; If less than 3AH, it’s 0–9
SUI 37H         ; Else subtract 37H for A–F
JMP STOREL

SUB30L: SUI 30H ; Subtract 30H for 0–9 conversion
STOREL: MOV B, A ; Store lower nibble in B

; --- Combine upper and lower nibbles ---
MOV A, C        ; Get upper nibble
RLC             ; Shift left 4 times
RLC
RLC
RLC
ADD B           ; Add lower nibble
OUT 02H         ; Output final hexadecimal number

HLT             ; Stop program
END
## Algorithm:
1.	Load the first ASCII digit from memory location 4200H.
2.	Convert it to hexadecimal by subtracting 30H (if it's a number) or 37H (if it's a letter A-F).
3.	Load the second ASCII digit from memory location 4201H and repeat the process.
4.	Combine the upper and lower nibbles to form a hexadecimal number.
5.	Store the result in memory location 4300H.
## Output:
<img width="1849" height="831" alt="image" src="https://github.com/user-attachments/assets/81256d16-17f5-4287-9678-0553d7477524" />
Input Ports:
 00H → ASCII of upper nibble
 01H → ASCII of lower nibble
Output Port:
 02H → Hexadecimal number (combined 8-bit value)
•	The hexadecimal equivalent of the ASCII input will be stored in memory location 4300H.

## Result:
The 8085 microprocessor successfully converts hexadecimal numbers to ASCII and vice versa, storing the results in memory.
