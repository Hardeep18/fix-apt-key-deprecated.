#Retrieve the list of known keys:

```
apt-key list

```

#Find the all groupings of hexadecimal characters that have 1 or 2 spaces in front of them, and are 4 characters long. Get the collection of those that have at 10 groupings per line. This provides the full key signature.

```
grep -E "(([ ]{1,2}(([0-9A-F]{4}))){10})"
```

#Trim away (delete) all spaces on each line found, so that key signature is unbroken by white space:

```
tr -d " "
```
#Grab the last 8 characters of each line:

```
grep -E "([0-9A-F]){8}\b"
```
#Now we have a collection of key suffixes, each 8 characters in length.

#Cycle through each key suffix, placing the current suffix in the KEY variable:

```
for KEY in $(…); do
```
#Assign the last 8 characters to the variable K:
```
K=${KEY:(-8)};
```
#Export the key that matches the signature in K and pass/pipe it to gpg to properly store it:
```
apt-key export $K | sudo gpg --dearmour -o /etc/apt/trusted.gpg.d/imported-from-trusted-gpg-$K.gpg

```
#Loop until all keys are processed.

###Enjoy no more deprecation warnings.
