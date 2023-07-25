# EC binary modules support
When ECX stumbles on an EC-module, it immediately translates it 
(internally) to the ECX module format before using it further.

ECX may place the resulting module in the cache as any other module, 
if the cache has not been deactivated.

## Notes:

Methods from EC / CreativE binary modules cannot be used.

CreativE modules:

* New internal functions are not implemented in ECX and will give an error 
   message.

* New internal globals will be NIL. It isn't possible for ECX to see if a 
   module is using these, so no error message will appear.

* New V51: CreativE modules using the internal global "__pool" now works.