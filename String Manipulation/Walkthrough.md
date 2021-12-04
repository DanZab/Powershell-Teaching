# String Manipulation
Often you will have a string value that contains a word or phrase you need to extract or manipulate. This section goes over the methods that are used in powershell to work with strings.

## Methods covered in this section
  - .Trim()
  - .Split()
  - .Replace()
  - .SubString()
  
## Methods vs Commands
You will notice that this section covers Methods instead of Powershell commands. Methods are unique from commands in that they are an action that are associated with a type of Object in powershell. You can find more technical information [here](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_methods?view=powershell-7.2).

For example, in the case of strings, the type of object is a string. And the actions (methods) that we cover in this section are associated with any type of string.

For these string methods, there are two ways to invoke the method:
  - You can use a dot (.) operator at the end of a string object like `$MyString.Split(",")`
  - You can use it similar to the way you add a parameter to a function like `$MySring -Split (",")`

## Walkthrough
This section will go over the basics of each method, these notes are also included in the comments of each .ps1 file

### .Trim()
Trim is used when you are retrieving a string that might include a leading or trailing space " " character that will interfere with your ability to use that string elsewhere in your script.

#### Trim Example 1
Let's say you have a form where an Active Directory user is entering their username, and that form triggers a powershell script with an LDAP query once the form is submitted:

```
# The user input from the form is $InputUser
$User = $InputUser
$LDAPInfo = Get-ADUser -LDAPFilter "(&(sAMAccountName=$User)(objectClass=user))"
```

In this case, if the user accidentally adds a trailing space, it would break the LDAP query because a username with a trailing space like "sampleusername " would return no result.

In our sample script, you can prevent any issues with leading or trailing spaces easily by using `$User = $InputUser.Trim()` instead.

#### Trim Example 2
Sometimes you pull data that is formatted as a fixed-width table in another source but comes across in powershell as a single string for each line, something like:

```
192.168.1.1      VLAN001
192.168.100.100  VLAN002
```

You could use .SubString() to isolate your data in each column, but you would end up with trailing spaces after the shorter values. From the example above, you would get one value as `'192.168.1.1    '` and the next as `'192.168.100.100'`. Using .Trim() would remove the spaces, allowing you to query using that value elsewhere without the risk of it being invalidated by space characters.

```
$LineItem = "192.168.1.1      VLAN001"
$Column1 = $LineItem.SubString(0,16).Trim()
$Column2 = $LineItem.SubString(16,$LineItem.Length).Trim()

# $Column1 now returns '192.168.1.1' instead of '192.168.1.1     '
```


