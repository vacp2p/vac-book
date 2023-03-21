# Napkin math for Community description message sizes

1. Number of chats/channels: 25
2. Strings: 32 bytes (ens_name,  display_name, magnet_uri, etc)
3. All keys: 32 bytes
4. Each member has a profile image
5. Each member has socials
6. Each member is granted access to all chats

Fixed overhead:

organization_id + clock + name + description + magnet_uri + permissions + chats + organization_identity + encryption_key + (ChatIdentity fields) ~ 728 bytes

Variable overhead:

number_of_members * ((1 grant * number_of_chats)+ 1 image + 1 social link)

# Calculations

- 100 members
    - variable overhead: 104000 
    - total size: 104728 bytes = 104.78 kb

- 1000 members
    - variable overhead: 1040000
    - total size: 104728 bytes = 1.04 mb

- 10000 members
    - variable overhead: 10400000
    - total size: 104728 bytes = 10.4 mb

After accessing telemetry =>

- 139 members (math)
    - variable overhead: 144560
    - total size: 145288 bytes = 145.28 kb

- 139 members (actual)
    - variable overhead: ?
    - total size: 346049 bytes = 346 kb

At approximately 401 members, we will cross the configured message size (1mb). Which means, the following is napkin math for 1000 and 10000 members respectively

- 1000 members
    - total size: 2.489 mb

- 10000 members
    - total size: 24.89 mb
