# Numeric PIN Ciphering

## Introduction

PIN numbers are a common form of security used to protect sensitive information on electronic devices. They also serve as a form of security with physical devices such as locks and safes.

This describes types of PIN numbers and their properties, as well as methods of ciphering and deciphering PIN numbers.

## Types of PIN Numbers

### Numeric PIN Numbers

Numeric PIN numbers are the most common form of PIN numbers. They consist of a series of numbers, typically 4 to 6 digits in length. The lengths of numeric PIN numbers can vary depending on the system or device they are used with. Generally the longer the PIN number, the more secure it is. Lengths of 4 and above are considered secure for most applications. 

A length of 3 is considered insecure but is still used in some applications, as a 3-digit PIN number has 1000 possible combinations, requiring a brute force attack to try all 1000 combinations, which is feasible for a single person to manually brute force in a reasonable amount of time. A length of 4 has 10,000 possible combinations, which is not feasible for a single person to manually brute force in a reasonable amount of time, approximately 10,000 seconds or 2.7 hours, but is feasible for even a low powered computer to brute force in only a few seconds.

A length of 5 has 100,000 possible combinations, which is not feasible for a low powered computer to brute force in a reasonable amount of time. A length of 6 has 1,000,000 possible combinations, which is not feasible for a high powered computer to brute force in a reasonable amount of time.

When a PIN number is limited to a specific set of numbers, such as 0-9, the number of possible combinations is equal to the number of possible values raised to the power of the length of the PIN number. For example, a 4-digit PIN number with values 0-9 has 10^4 or 10,000 possible combinations.

Generally, a pin number of length 6 and below is only pratical if it utilizes at least 10 possible values, such as 0-9, as the number of possible combinations is not feasible for a computer to brute force in a reasonable amount of time.

When a PIN length is particularly long, such as 10 characters or more, if it utilizes less than 10 possible values, such as only 0-3, the number of possible combinations is not feasible for a computer to brute force in a short amount of time. This may be used in some applications where one wishes to simplify the PIN for the user, while still meeting a minimum level of security. This pattern might be less likely to be detected as other patterns that utilize all possible values but are more predictable such as 0123456789 vs 0230303132.

Pins of length 4 are generally very convenient and suitably secure for most applications. Pins of length 6 are generally very secure and are suitable for applications that require a higher level of security, but can be less convenient for the user to enter particularly if they are required to enter the PIN frequently.

### Alphanumeric PIN Numbers

Alphanumeric PIN numbers consist of a series of numbers and letters. They are generally more secure than numeric PIN numbers of the same length, as they have more possible combinations. For example, a 4-digit alphanumeric PIN number with values 0-9 and A-Z has 36^4 or 1,679,616 possible combinations.

When a pin number is strictly based on numerical values, it's possible to utilize external systems to associate the numbers with letters, such as the letters on a phone keypad. This can be used to create a more secure alphanumeric PIN number that is easier to remember, such as "HELLO" which corresponds to "43556" on a phone keypad. A password like this is considered ciphered, as it is not immediately obvious to an attacker that the password is based on the letters on a phone keypad.

### Ciphering and Deciphering PIN Numbers

The options to cipher and decipher PIN numbers can change based on the overall length.

We will focus on describing methods for the different lengths of numeric PIN numbers.

#### 3-Digit PIN Numbers

#### 4-Digit PIN Numbers

#### 5-Digit PIN Numbers

#### 6-Digit PIN Numbers