Extension to be merged into life-cycle-manager

This new LCM code is able to load/update the bootloader from github.  
The new bootloader is able to count the amount of short power cycles (<1s)  
The boot loader conveys the count to the loaded code using the rtc.temp_rom value  
User code is allowed the values from 1-4  
- 1  : this is a normal boot
- 2-4: users choice in user code (communicate 3 to user or risk a bit and use 2, 3 and 4 separatly)

If count > 4 then it launches LCM otamain.bin in rom[1]  

For these values the behaviour is controlled through the sysparam int8 `ota_count_step` which defaults to 3 if it is not set.
The reason to jump with 3 at a time is to reduce the user misscounting and triggering something else than intended.  
If `ota_count_step==3`
- 5-7: check for new code  (communicate 6 to user)
- 8-10: erase wifi info (communicate 9 to user)
- 11-13: factory reset ( (communicate 12 to user)
- 14-16: factory reset and join LCM_beta (communicate 15 to user)


If `ota_count_step==2`
- 5-6: check for new code  (communicate 6 to user)
- 7-8: erase wifi info (communicate 8 to user)
- 9-10: factory reset ( (communicate 10 to user)
- 11-12: factory reset and join LCM_beta (communicate 12 to user)

If `ota_count_step==1`
- 5: check for new code  (communicate 5 to user)
- 6: erase wifi info (communicate 6 to user)
- 7: factory reset ( (communicate 7 to user)
- 8: factory reset and join LCM_beta (communicate 8 to user)

Other `ota_count_step` values will be interpreted as 3