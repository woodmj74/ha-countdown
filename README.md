# ha-countdown

My variation of https://github.com/mf-social/ps-date-countdown (*which by the way is awesome!*).

This was done by using mf-socials script and hacking with the use of Google, I've never written a real line of code before so appreciate any suggestions!!

For my needs, I wanted a sensor as well which reminded me 4 and 2 weeks out a birthday coming up. To do this without having to reference each individual birthday sensor, created a group with these then expanded it in the sensor and if matches were made for the time window added the birthday in an attribue which was then included in a message.

Given the dates don't change, added these to the script so that I didn't need to both update the call to the script to add a new set of parameters, and add the subsquent group to monitor them.

As its only me who maintains this, it's not hassle.

Then had this running behind the sensor.
```
- id: '1602361830298'
  alias: Reminder - Birthdays Within Four Weeks
  description: ''
  trigger:
  - platform: state
    entity_id: sensor.birthdays_four_weeks
  condition:
  - condition: not
    conditions:
    - condition: state
      entity_id: sensor.birthdays_four_weeks
      state: '0'
  - condition: template
    value_template: '{{ trigger.from_state.state != trigger.to_state.state }}'
  action:
  - service: notify.mike
    data:
      message: '{%- set ns = namespace(message = ''The following birthday(s) are within
        30 days:\n'') -%}  {%- set ns.message = ns.message +  state_attr(''sensor.birthdays_four_weeks'',''upcoming'').split(''|'')
        | join('' days \n'')  -%} {{ ns.message }}'
      title: '** Birthday Reminder **'
```      
      
