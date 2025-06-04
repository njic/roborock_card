My roborock S7 card. Using Roborock integration sensors.

Make sure you have config in your configuration.yaml to get it working

roborock_config.yaml contains:
1. input_boolean for each room and one more that toggles between 1x and 2x repeat
2. toggle all rooms script
3. start cleaning script

roborock_card.yaml contains card yaml code


![Cleaning](screenshots/1.png)
![Idle](screenshots/2.png)
![Idle, no error](screenshots/3.png)

### How to use this card
Note this is a "quickly" writen and not tested guide (and card), made by non-expert:

1. Install HACS and all necessary cards ([card-mod](https://github.com/thomasloven/lovelace-card-mod) and [button-card](https://github.com/custom-cards/button-card). Layout card and Picture elements card are in defeault HA system if I'm not wrong.
2. Install Roborock integration [check docs](https://www.home-assistant.io/integrations/roborock/)
3. Fetch room numbers (Developer tools -> Actions -> Roborock: Get maps) and remember that. It should look like this:

```
vacuum.s7_max_ultra:
  maps:
    - flag: 0
      name: Map 0
      rooms:
        "16": Living room
        "17": Study
        "18": Kitchen
        "19": Bedroom
        "20": Shower
        "21": Balcony
        "22": Hall
        "23": Bathroom
```
4. Check entity names (Developer tools -> States):

sensor.s7_max_ultra_status  (charging = Ready, otherwise = Cleaning)
![image](https://github.com/user-attachments/assets/d356f815-53a8-4d02-a48d-f488e86c4f3f)

sensor.s7_max_ultra_current_room
![image](https://github.com/user-attachments/assets/1eb305a0-98d6-43d4-940d-eb4ae1548f3f)

vacuum.s7_max_ultra (fan speed and running actions for play, pause, stop, return to dock, locate, set fan speed)
![image](https://github.com/user-attachments/assets/d446af2d-d898-4b86-bcf6-c4a41e4c2e37)

sensor.s7_max_ultra_cleaning_area
![image](https://github.com/user-attachments/assets/d80def6a-6500-4e56-996d-6f871d7fa3c1)

sensor.s7_max_ultra_cleaning_progress
![image](https://github.com/user-attachments/assets/f3ea3b6a-81b2-4a91-a3d3-56661573603b)

sensor.s7_max_ultra_last_clean_end
![image](https://github.com/user-attachments/assets/401005eb-1d01-4bf5-8083-b065df68f70b)

sensor.s7_max_ultra_vacuum_error
![image](https://github.com/user-attachments/assets/8c5e769b-6dbd-4492-8a78-ff2c6faa464f)

sensor.s7_max_ultra_dock_error
![image](https://github.com/user-attachments/assets/3508d603-9b69-4c98-a43d-cddc7b64987f)

sensor.s7_max_ultra_battery
![image](https://github.com/user-attachments/assets/c77f7704-127a-4384-9b88-f7d247c8c1c2)

image.s7_max_ultra_map_0
![image](https://github.com/user-attachments/assets/e36c196a-0168-498b-8a00-597a6dba7f9a)

sensor.s7_max_ultra_total_cleaning_area
![image](https://github.com/user-attachments/assets/805a3da9-6ff1-4a71-8aaf-3a1e450f1562)

sensor.s7_max_ultra_total_cleaning_time
![image](https://github.com/user-attachments/assets/e8617578-bd29-433b-b072-a218f1b89aa9)

5.a Add roborock_config.yaml to your configuration.yaml and restart HA
5.b Alternatively (to make it cleaner): create a folder "Packages" in root HA folder and add this to configuration.yaml:
```
homeassistant:
  packages: !include_dir_named packages
```
(and then your configuration will read any yaml file in that folder. Just save roborock_config.yaml in there)

6. Modify roborock_config.yaml if needed:
   a. each room should have its own input_boolean and then also one more boolean (clean_twice) that toggles between repeat x1 and x2. Names does not matter as long as you keep the same in card yaml
   b. update script clean_selected_rooms with correct input_boolean names and room numbers
   c. update script toggle_clean_booleans with correct room names

7. Create a new card (eddit HA dashboard and add a Manual Card), modify the code and paste it there.
   a. Make sure you update the code using correct entity names (step 3 and 4)
   b. To position the map correctly you can modify the width (like 726, change 140px value), postion (line 739 and 740) and zoom (line 741).
   c. If you have different number of rooms modify lines 330-455
      - if you have less than 7 rooms you can just delete custom:button-card that you dont need
      - if you have 8 rooms you can just add one more custom:button-card
      - if you have more than 8 (or less than 4) you have to do more styling to fit third row of rooms (line 4 and 334)

Hope it helps
