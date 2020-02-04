Extension to be merged into life-cycle-manager

This new LCM code is able to load/update the bootloader.  
The loaded bootloader is able to count the amount of short power cycles (<1s)  
The boot loader only conveys the count using the rtc.temp_rom value  
If count > 4 then it launches LCM otamain in rom[1]  
LCM itself does the logic and the user code is allowed the values from 1-4  
- 1   : this is a normal boot
- 2-4: users choice in user code (communicate 3 to user or risk a bit and use 2, 3 and 4 separatly)
- 5-7: check for new code  (communicate 6 to user)
- 8-10: erase wifi info (communicate 9 to user)
- 11-13: factory reset ( (communicate 12 to user)
- 14-16: factory reset and join LCM_beta (communicate 15 to user)

The reason to jump with 3 at a time is to reduce the user miscounting and triggering something else than intended