# Archiecture-
.data
InputName: .space 40 ## array 
File_Content: .space 1000
only_Alpha_file: .space 1000 ## input for decription 
output_file: .space 1000
shift_value: .word 1:100
message1: .asciiz "Enter E for encryption_function process or D if u want the decryption process: "
message2: .asciiz "\n?Write the name of the input file example input.txt "
message3: .asciiz "\nEnter the name of the Non_Alphabet file : "
message4:  .asciiz "The Shift Value = "
message5:  .asciiz "\nThe Program is End"
.text
.globl main
main: 
la $a0, message1
li $v0, 4  
syscall
li $v0, 12
syscall
beq $v0, 'E', encryption_function
beq $v0, 'D', decryption
j End_of_program
#############  encryption functions #####################3
encryption_function:
la $a0, message2
li $v0, 4	
syscall 
la $a0, InputName
li $a1, 40
li $v0, 8
syscall 
move $t2, $zero
Null:
lb $t0, InputName($t2)
beq $t0, 10, replace
addiu $t2, $t2, 1
j Null
replace:
sb $zero, InputName($t2)
la $a0, InputName
li $v0, 13
li $a1, 0
syscall 
move $a0, $v0
blt $a0, 0, End_of_program
la $a1, File_Content
li $a2, 1000
li $v0, 14
syscall 
move $t0, $v0
li $v0, 16
syscall
move $t2, $zero
move $t3, $zero
la $a0, File_Content
la $a1, only_Alpha_file
remove_nonAlphabet: 
beq $t3, $t0, Convet_To_Lower
lb $t1, 0($a0) 
addiu $a0, $a0, 1
addiu $t3, $t3, 1
beq $t1, ' ', Add_to_only_Alpha_file
bgt $t1, 'Z', Small_Char
blt $t1, 'A', remove_nonAlphabet
j Add_to_only_Alpha_file
Small_Char:
bgt $t1, 'z', remove_nonAlphabet
blt $t1, 'a', remove_nonAlphabet
Add_to_only_Alpha_file:
sb $t1, 0($a1)
addiu $a1, $a1, 1
addiu $t2, $t2, 1
j remove_nonAlphabet
Convet_To_Lower:
move $t0, $zero
la $a0, only_Alpha_file
subiu $a0, $a0, 1
Loop:
beq $t0, $t3, shifted_value_function
addiu $a0, $a0, 1
addiu $t0, $t0, 1
lb $t1, 0($a0)
bge $t1, 'a', Loop
beq $t1, ' ', Loop
addiu $t1, $t1, 32
sb $t1, 0($a0)
j Loop
shifted_value_function:
la $a0, only_Alpha_file
la $a1, shift_value
move $t0, $zero
move $t2, $zero
move $t4, $zero
la $a0, only_Alpha_file
calculate_shift_number:
beq $t0, $t3, write_shift_value_function
lb $t1, 0($a0)
beq $t1, ' ', NewWord
addiu $a0, $a0, 1
addiu $t0, $t0, 1
addiu $t2, $t2, 1
j calculate_shift_number
NewWord:
sw $t2, 0($a1)
addiu $a1, $a1, 4
addiu $a0, $a0, 1
addiu $t0, $t0, 1
addiu $t4, $t4, 1
move $t2, $zero
j calculate_shift_number
write_shift_value_function:
sw $t2, 0($a1)
addiu $t4, $t4, 1
la $a0, shift_value
move $a1, $t4
move $t8, $t3
jal sort_function
la $a0, message4
li $v0, 4	
syscall
la $a1, shift_value
lw $a0, 0($a1)
li $v0, 1
syscall
move $t6, $a0
move $t0, $zero
la $a0, only_Alpha_file
la $a1, output_file
Add_shift_value:
beq $t0, $t8, read_output_file_name
lb $t1, 0($a0)
beq $t1, ' ', Store_to_file
add $t1, $t1, $t6
ble $t1, 'z', Store_to_file
subiu $t1, $t1, 26
Store_to_file:
sb $t1, 0($a1)
addiu $a0, $a0, 1
addiu $a1, $a1, 1
addiu $t0, $t0, 1
j Add_shift_value
read_output_file_name:
la $a0, message3
li $v0, 4	
syscall 
la $a0, InputName
li $a1, 40
li $v0, 8
syscall 
move $t2, $zero
Null_output:
lb $t0, InputName($t2)
beq $t0, 10, replac_out
addiu $t2, $t2, 1
j Null_output
replac_out:
sb $zero, InputName($t2)
la $a0, InputName
li $v0, 13
li $a1, 1
syscall 
move $a0, $v0
la $a1, output_file
move $a2, $t8
li $v0, 15
syscall 
move $t0, $v0
li $v0, 16
syscall
j End_of_program
############### Decryption function ################
decryption:
la $a0, message3
li $v0, 4	
syscall 
la $a0, InputName
li $a1, 40
li $v0, 8
syscall 
move $t2, $zero
Null_In_dec:
lb $t0, InputName($t2)
beq $t0, 10, replace_In_dec
addiu $t2, $t2, 1
j Null_In_dec
replace_In_dec:
sb $zero, InputName($t2)
la $a0, InputName
li $v0, 13
li $a1, 0
syscall 
move $a0, $v0
blt $a0, 0, End_of_program
la $a1, only_Alpha_file
li $a2, 1000
li $v0, 14
syscall 
move $t8, $v0
li $v0, 16
syscall
la $a0, only_Alpha_file
la $a1, shift_value
move $t0, $zero
move $t2, $zero
move $t4, $zero
la $a0, only_Alpha_file
calculate_shift_number_dec:
beq $t0, $t8, write_shift_value_function_dec
lb $t1, 0($a0)
beq $t1, ' ', NewWord_dec
addiu $a0, $a0, 1
addiu $t0, $t0, 1
addiu $t2, $t2, 1
j calculate_shift_number_dec
NewWord_dec:
sw $t2, 0($a1)
addiu $a1, $a1, 4
addiu $a0, $a0, 1
addiu $t0, $t0, 1
addiu $t4, $t4, 1
move $t2, $zero
j calculate_shift_number_dec
write_shift_value_function_dec:
sw $t2, 0($a1)
addiu $t4, $t4, 1
la $a0, shift_value
move $a1, $t4
jal sort_function
la $a0, message4
li $v0, 4	
syscall
la $a1, shift_value
lw $a0, 0($a1)
li $v0, 1
syscall
move $t6, $a0
move $t0, $zero
la $a0, only_Alpha_file
la $a1, output_file
Sub_shift_value_to_text:
beq $t0, $t8, read_output_file_name_dec
lb $t1, 0($a0)
beq $t1, ' ', Store_to_file_dec
sub $t1, $t1, $t6
bge $t1, 'a', Store_to_file_dec
addiu $t1, $t1, 26
Store_to_file_dec:
sb $t1, 0($a1)
addiu $a0, $a0, 1
addiu $a1, $a1, 1
addiu $t0, $t0, 1
j Sub_shift_value_to_text
read_output_file_name_dec:
la $a0, message2
li $v0, 4	
syscall 
la $a0, InputName
li $a1, 40
li $v0, 8
syscall 
move $t2, $zero
Null_output_dec:
lb $t0, InputName($t2)
beq $t0, 10, replac_out_dec
addiu $t2, $t2, 1
j Null_output_dec
replac_out_dec:
sb $zero, InputName($t2)
la $a0, InputName
li $v0, 13
li $a1, 1
syscall 
move $a0, $v0
la $a1, output_file
move $a2, $t8
li $v0, 15
syscall 
move $t0, $v0
li $v0, 16
syscall
End_of_program:
la $a0, message5
li $v0, 4	
syscall 
li $v0, 10
syscall
sort_function: # $a0 = &A, $a1 = n
do: addiu $a1, $a1, -1 # n = n-1
blez $a1, L2 # branch if (n <= 0)
move $t0, $a0 # $t0 = &A
li $t1, 0 # $t1 = swapped = 0
li $t2, 0 # $t2 = i = 0
for: lw $t3, 0($t0) # $t3 = A[i]
lw $t4, 4($t0) # $t4 = A[i+1]
bge $t3, $t4, L1 # branch if (A[i] => A[i+1])
sw $t4, 0($t0) # A[i] = $t4
sw $t3, 4($t0) # A[i+1] = $t3
li $t1, 1 # swapped = 1
L1: addiu $t2, $t2, 1 # i++
addiu $t0, $t0, 4 # $t0 = &A[i]
bne $t2, $a1, for # branch if (i != n)
bnez $t1, do # branch if (swapped)
L2: jr $ra # return to caller
	
