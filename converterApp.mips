.data
welcome: 	.asciiz " -- Welcome to the Number Converter -- "
selection: 	.asciiz "\n\nMake a Selection from 1-4 \n"
choices:   	.asciiz "1.) Binary to decimal and hexidecimal \n2.) Hexadecimal to Binary and decimal \n3.) Decimal to Binary and hexadecimal \n4.) Exit\n\n"
userInput: 	.space 32
enterBin:	.asciiz "\nEnter Binary Number:"
enterDec:	.asciiz "\nEnter Decimal Number:"
enterHex:	.asciiz "\nEnter Hexadecimal Number:"
isBinary:	.asciiz "\nBinary Number is:  " 
isDecimal:   	.asciiz "\nDecimal Number is:  "
isHexadecimal:	.asciiz "\nHexadecimal Number is:  "
invalidBinary:	.asciiz "\nInvalid Binary Input"
invalidDec:	.asciiz "\nInvalid Decimal Input"
invalidHex:     .asciiz "\nInvalid Hexadecimal Input"

.text 
main: 
    la $a0, welcome
    li $v0, 4 
    syscall

    jal menu
    li $v0, 10
    syscall

menu:  
    addi $sp, $sp, -4
    sw $ra, 0($sp)

menuLoop:
    la $a0, selection
    li $v0, 4 
    syscall
    
    la $a0, choices
    li $v0, 4 
    syscall
    
    li $v0, 5
    syscall
    
    li $t1, 1
    li $t2, 2
    li $t3, 3
    li $t4, 4
    
    move $t7, $v0
    
    beq $t7, $t1, option1
    beq $t7, $t2, option2
    beq $t7, $t3, option3
    beq $t7, $t4, endProgram
   
option1: 
    jal binaryToHexNDec
    j menuLoop

option2: 
    jal hexToBinaryNDec
    j menuLoop

option3:	
    jal decimalToBinaryNHex
    j menuLoop 

endProgram:
    lw $ra, 0($sp)	
    addi $sp, $sp, 4
    jr $ra
  
binaryToHexNDec:
    addi $sp, $sp, -4	
    sw $ra, 0($sp)
    li $v0, 4
    la $a0, enterBin
    syscall

    li $v0, 8
    la $a0, userInput
    li $a1, 32
    syscall 

    la $a0, userInput
    jal binaryToDecimal
    bnez $v1, invalidBinInput
    move $s1, $v0
    la $a0, isDecimal
    li $v0, 4
    syscall 
    
    move $a0,$s1
    li $v0,1
    syscall

    la $a0, isHexadecimal
    li $v0, 4
    syscall 
    
    move $a0, $s1
    li $v0, 34
    syscall

invalidBinInput:
    lw $ra, 0($sp)
    addi $sp, $sp, 4
    jr $ra

hexToBinaryNDec:
    addi $sp, $sp, -4
    sw $ra, 0($sp)

    la $a0, enterHex
    li $v0, 4
    syscall 
    li $v0, 8
    la $a0, userInput
    li $a1, 32
    syscall 
    
    la $a0, userInput
    jal hexadecimalToDecimal
    bnez $v1, invalHex
    move $s1, $v0
    
    li $v0, 4
    la $a0, isBinary
    syscall

    move $a0, $s1
    li $v0, 35
    syscall
    
    la $a0, isDecimal
    li $v0, 4
    syscall 

    move $a0, $s1
    li $v0, 1
    syscall    

invalHex:
    lw $ra, 0($sp)
    addi $sp, $sp, 4
    jr $ra

hexadecimalToDecimal: 
    addi $sp, $sp, -4	
    sw $s0, 0($sp)
    li $v0, 0
    li $s4, 0xA
    li $v1, 0
    move $v0,$zero
    li $t3, 6   
    li $t4, 10  
    li $t5, 0x30 #'0'
    li $t6, 0x41 #'A'
    li $t7, 0x61 #'a'
    
hexStringToDec:
    lb $t0, 0($a0)  
    beq $t0, $s4, branch  
    sub $t1, $t0, $t5 
    bltz $t1, invalidHexInput  
    sub $t2, $t1, $t4         
    bltz $t2, validHex   
    sub $t1, $t0, $t6    
    bltz $t1, invalidHexInput     
    sub $t2, $t1, $t3   
    bltz $t2, toHex   
    sub $t1, $t0, $t7 
    bltz $t1, invalidHexInput   
    sub $t2, $t1, $t3  
    bltz $t2, toHex    
    j invalidHexInput
    
toHex:
    addi $t1, $t1, 10     
  
validHex:
    sll $t2, $v0, 4       
    add $v0, $t2, $t1     
    addi $a0, $a0, 1           
    j    hexStringToDec  
           
branch:
    lw $s0, 0($sp)	
    addi $sp, $sp, 4
    jr $ra                             
 
invalidHexInput:
    la $a0, invalidHex
    li $v0, 4
    syscall
    li $v1, 1
    j branch

binaryToDecimal: 
    li $s2, 0xA
    li $s3, 1
    li $v1, 0
    addi $sp, $sp, -4	
    sw $s0, 0($sp)
    move $v0, $zero  
   
binStringToDec:
    lb $s0, 0($a0)    
    beq $s0, $s2, exitLoop 
    sll $v0, $v0, 1	
    subi $s0, $s0, 48	
    add $v0, $v0, $s0	
    bltz $s0, invBin	
    bgt $s0, $s3,invBin	
    addi $a0, $a0, 1	
    j binStringToDec

invBin:
    la $a0, invalidBinary
    li $v0, 4
    syscall
    li $v1, 1
    j exitLoop
   
exitLoop:
    lw $s0, 0($sp)	
    addi $sp, $sp, 4
    jr $ra	

decimalToBinaryNHex:	
    addi $sp, $sp, -4
    sw $ra, 0($sp)

    la $a0, enterDec
    li $v0, 4
    syscall

    li $v0, 8
    la $a0, userInput
    li $a1, 32
    syscall 

    jal stringToDecimal 
    bnez  $v1, invalDec 
    move $s0, $v0

    li $v0, 4
    la $a0, isBinary
    syscall
    move $a0, $s0 
    li $v0, 35
    syscall

    la $a0, isHexadecimal
    li $v0, 4
    syscall 
   
    move $a0, $s0 
    li $v0, 34 
    syscall

invalDec:
    lw $ra, 0($sp)
    addi $sp, $sp, 4
    jr $ra

stringToDecimal:
    addi $sp, $sp, -4	 
    sw $s0, 0($sp)
    li $s3, 9
    li $v1, 0
    li $v0, 0
    li $s4, 0xA
    addi $t5, $zero, 0x30

decStringToDec:
    lb $t0, 0($a0)  
    beq $t0, $s4, exitLoop2
    sub $t1, $t0, $t5
    bltz $t1, decInvalid 
    bgt $t1, $s3, decInvalid

    mul $t2, $v0, 10      
    add $v0, $t2, $t1    
    addi $a0, $a0, 1           
    j decStringToDec     
                      
exitLoop2:
    lw $s0, 0($sp)	
    addi $sp, $sp, 4
    jr $ra   
                            
decInvalid:
    la $a0, invalidDec
    li $v0, 4
    syscall
    
    li $v1, 1
    j exitLoop2
