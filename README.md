# utl_save_binary_data_in_macro_variables
Convert your binary data to base64 or hex strings. You can even use a delimiter. Big data analytics. Machine learning, SAS Perl Python R.

    ```  Save binary data in macro variables using base64 or hex (without a delimiter, could add a delimiter)                                                         ```
    ```                                                                                                                                                               ```
    ```  Does not exactly answer the ops question but is safe                                                                                                         ```
    ```  You can add a delimiter, but I prefer fixed lengths.                                                                                                         ```
    ```                                                                                                                                                               ```
    ```  I will create the eqivalent macro variable for 3,276 values of length 10, fixed length no delimiters,                                                        ```
    ```  Slightly more complex with a delimiter.                                                                                                                      ```
    ```                                                                                                                                                               ```
    ```  The actual macro variable will have length 65,520, max macro variable length is 65,535.                                                                      ```
    ```  You may do better with base64, also if you must you can use a comma delimiter.                                                                               ```
    ```  I may have bug due to how SAS handles 200 byte boundaries when you go from 32,756                                                                            ```
    ```  tp 65,535. It is fixable.                                                                                                                                    ```
    ```                                                                                                                                                               ```
    ```  SAS-L Delimiting values in a Macro Variable (safely-ish)                                                                                                     ```
    ```                                                                                                                                                               ```
    ```     WORKING CODE                                                                                                                                              ```
    ```                                                                                                                                                               ```
    ```     * creare a string with all possible binary characters;                                                                                                    ```
    ```     do rank=0 to 255;                                                                                                                                         ```
    ```      chr=cats(chr,put(byte(rank),$hex2.));                                                                                                                    ```
    ```     end;                                                                                                                                                      ```
    ```     mac=cats(repeat(chr,126),repeat('00'x,247));                                                                                                              ```
    ```                                                                                                                                                               ```
    ```     call symputx('mac1',mac);                                                                                                                                 ```
    ```     call symputx('mac2',mac);                                                                                                                                 ```
    ```                                                                                                                                                               ```
    ```     %let mac=&mac1.&mac2;   * ALMOST 64K MACRO VARIABLE;                                                                                                      ```
    ```                                                                                                                                                               ```
    ```     * lets load the ten digit number 1234567890 from the messy 64k macro variable;                                                                            ```
    ```                                                                                                                                                               ```
    ```     * proof of concept. Extract 10 digit number from 3,276 10 digit strings  - could have been IEE float in hex;                                              ```
    ```                                                                                                                                                               ```
    ```     %let digTenHex=%substr(&mac,97,20);                                                                                                                       ```
    ```     %put &=digTenHex;                                                                                                                                         ```
    ```                                                                                                                                                               ```
    ```     data digTenFloat;                                                                                                                                         ```
    ```        Num = input ("&digTenHex"x,10.);                                                                                                                       ```
    ```      put num=;                                                                                                                                                ```
    ```                                                                                                                                                               ```
    ```     WORK.DIGTENFLOAT total obs=1                                                                                                                              ```
    ```                                                                                                                                                               ```
    ```      Obs       NUM                                                                                                                                            ```
    ```                                                                                                                                                               ```
    ```       1     123456789                                                                                                                                         ```
    ```                                                                                                                                                               ```
    ```                                                                                                                                                               ```
    ```  HAVE  (macro string of hex digits which is 64,560 characters long)                                                                                           ```
    ```                                                                                                                                                               ```
    ```                                                                                                                                                               ```
    ```      MAC=                                                                                                                                                     ```
    ```      000102030405060708090A0B0C0D0E0F101112131415161718191A1B1C1D1E1F20                                                                                       ```
    ```      35455565758595A5B5C5D5E5F606162636465666768696A6B6C6D6E6F707172737                                                                                       ```
    ```      292A2B2C2D2E2F303132333435363738393A3B3C3D3E3F40414243444546474849                                                                                       ```
    ```      E7F000102030405060708090A0B0C0D0E0F101112131415161718191A1B1C1D1E1                                                                                       ```
    ```      5455565758595A5B5C5D5E5F606162636465666768696A6B6C6D6E6F7071727374                                                                                       ```
    ```      92A2B2C2D2E2F303132333435363738393A3B3C3D3E3F404142434445464748494                                                                                       ```
    ```      7F000102030405060708090A0B0C0D0E0F101112131415161718191A1B1C1D1E1F                                                                                       ```
    ```      ...                                                                                                                                                      ```
    ```                                                                                                                                                               ```
    ```  MAC=000102030405060708090A0B0C0D0E0F101112131415161718191A1B1C1D1E1F20212223242526272                                                                        ```
    ```                                                                                                                                                               ```
    ```              Position                                                                                                                                         ```
    ```                  97                                                                                                                                           ```
    ```                   0 1 2 3 4 5 6 7 8 9                                                                                                                         ```
    ```  8292A2B2C2D2E2F 30313233343536373839 3A3B3C3D3E3F404142434445464748494A4B4C4D4E4F5051525                                                                     ```
    ```                                                                                                                                                               ```
    ```                                                                                                                                                               ```
    ```  WANT                                                                                                                                                         ```
    ```                                                                                                                                                               ```
    ```     The 10 disgit number I stored in substr(&mac,95,20)                                                                                                       ```
    ```                                                                                                                                                               ```
    ```      NUM=0123456789                                                                                                                                           ```
    ```                                                                                                                                                               ```
    ```  *                _               _       _                                                                                                                   ```
    ```   _ __ ___   __ _| | _____     __| | __ _| |_ __ _                                                                                                            ```
    ```  | '_ ` _ \ / _` | |/ / _ \   / _` |/ _` | __/ _` |                                                                                                           ```
    ```  | | | | | | (_| |   <  __/  | (_| | (_| | || (_| |                                                                                                           ```
    ```  |_| |_| |_|\__,_|_|\_\___|   \__,_|\__,_|\__\__,_|                                                                                                           ```
    ```                                                                                                                                                               ```
    ```  ;                                                                                                                                                            ```
    ```                                                                                                                                                               ```
    ```  data _null_;                                                                                                                                                 ```
    ```    length chr $256. mac $32760;                                                                                                                               ```
    ```     do rank=0 to 255;                                                                                                                                         ```
    ```      chr=cats(chr,put(byte(rank),$hex2.));                                                                                                                    ```
    ```     end;                                                                                                                                                      ```
    ```     mac=cats(repeat(chr,126),repeat('00'x,247));                                                                                                              ```
    ```     call symputx('mac1',mac);                                                                                                                                 ```
    ```     call symputx('mac2',mac);                                                                                                                                 ```
    ```  run;quit;                                                                                                                                                    ```
    ```                                                                                                                                                               ```
    ```  %let mac=&mac1.&mac2;                                                                                                                                        ```
    ```                                                                                                                                                               ```
    ```  *          _       _   _                                                                                                                                     ```
    ```   ___  ___ | |_   _| |_(_) ___  _ __                                                                                                                          ```
    ```  / __|/ _ \| | | | | __| |/ _ \| '_ \                                                                                                                         ```
    ```  \__ \ (_) | | |_| | |_| | (_) | | | |                                                                                                                        ```
    ```  |___/\___/|_|\__,_|\__|_|\___/|_| |_|                                                                                                                        ```
    ```                                                                                                                                                               ```
    ```  ;                                                                                                                                                            ```
    ```                                                                                                                                                               ```
    ```  * lets load the ten digit number 1234567890 from the messy 64k macro variable;                                                                               ```
    ```                                                                                                                                                               ```
    ```  %let digTenHex=%substr(&mac,97,20);                                                                                                                          ```
    ```                                                                                                                                                               ```
    ```  data digTenFloat;                                                                                                                                            ```
    ```    Num = input ("&digTenHex"x,10.);                                                                                                                           ```
    ```    put num=;                                                                                                                                                  ```
    ```  run;quit;                                                                                                                                                    ```
    ```                                                                                                                                                               ```
    ```                                                                                                                                                               ```
    ```
