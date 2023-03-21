# Status Telemetry Analysis

> All the following data comes from Status desktop clients running version `0.10.0rc`

> There are ~30 members of the status community running this version


## Methodology

The [Status superset](https://superset.infra.status.im/superset/dashboard/13/?native_filters_key=IaOrPqizciMolxZRbRhk0vAFWWeVt8jxeuTzyiawNKIXpuJRqn5E9KvSnvOoUgGa) runs upon the pg database that has all the telemetry data.

These queries were run directly on the database, with the following index added -

```sql=
CREATE INDEX idx_receivedmessages_messagetype_messagesize_chatid
ON receivedmessages (messagetype, messagesize, chatid);
```

which helps query times re: sizes of each `messagetype` being broadcasted

## Main query

`messagetypes` ordered by size, descending -

```sql
SELECT 
  messagetype, 
  AVG(messagesize) 
FROM 
  receivedmessages 
WHERE 
  messagesize > 0 
GROUP BY 
  messagetype 
ORDER BY 
  AVG(messagesize) DESC 
LIMIT 
  30;
```
which yields
```
            messagetype             |          avg
------------------------------------+-----------------------
 MEMBERSHIP_UPDATE_MESSAGE          |    67950.887645733805
 COMMUNITY_DESCRIPTION              |    48434.633391221031
 UNKNOWN                            |    32869.262119978472
 BACKUP                             |    29259.738297689599
 COMMUNITY_REQUEST_TO_JOIN_RESPONSE |    24488.108695652174
 SYNC_PROFILE_PICTURE               |    19550.000000000000
 CONTACT_CODE_ADVERTISEMENT         | 7421.5858845617061659
 CHAT_IDENTITY                      | 6109.9454022988505747
 CHAT_MESSAGE                       | 4670.8508814547473626
 SYNC_INSTALLATION_COMMUNITY        | 3597.6666666666666667
 CONTACT_UPDATE                     | 1981.9735152487961477
 EDIT_MESSAGE                       | 1616.5792079207920792
 ACCEPT_CONTACT_REQUEST             | 1337.4227272727272727
 SYNC_CHAT_MESSAGES_READ            | 1114.2974738675958188
 SYNC_INSTALLATION_CONTACT          | 1094.3000000000000000
 RETRACT_CONTACT_REQUEST            |  945.1025641025641026
 SYNC_ACTIVITY_CENTER_READ          |  842.8750000000000000
 REQUEST_CONTACT_VERIFICATION       |  773.0000000000000000
 SYNC_CHAT_REMOVED                  |  754.0000000000000000
 SYNC_CONTACT_REQUEST_DECISION      |  746.5000000000000000
 DELETE_MESSAGE                     |  735.8549450549450549
 COMMUNITY_ARCHIVE_MAGNETLINK       |  636.1341371514694800
 EMOJI_REACTION                     |  606.1040987716219604
 PIN_MESSAGE                        |  483.2560000000000000
 PAIR_INSTALLATION                  |  459.7571428571428571
 STATUS_UPDATE                      |  338.4547629848038104
 PUSH_NOTIFICATION_QUERY            |  137.0000000000000000
 COMMUNITY_REQUEST_TO_JOIN          |  125.1176470588235294
 COMMUNITY_CANCEL_REQUEST_TO_JOIN   |  122.0000000000000000
 COMMUNITY_REQUEST_TO_LEAVE         |  112.0000000000000000
```
    
Ignoring `UNKNOWN`, we will run queries for the top 5, i.e

- MEMBERSHIP_UPDATE_MESSAGE
- COMMUNITY_DESCRIPTION
- BACKUP
- COMMUNITY_REQUEST_TO_JOIN_RESPONSE
- SYNC_PROFILE_PICTURE

## Queries

### 1. `MEMBERSHIP_UPDATE_MESSAGE`

1. Window of `MEMBERSHIP_UPDATE_MESSAGE` sizes sent by different groups(?) -

    ```sql
    SELECT 
      left(chatid, 20) as trunc_chat_id, 
      messagetype, 
      messagesize, 
      sentat 
    FROM 
      receivedmessages 
    WHERE 
      messagetype = 'MEMBERSHIP_UPDATE_MESSAGE' 
      AND messagesize > 0
    ORDER BY sentat DESC -- to get the latest sizes
    LIMIT 
      10;
    ```

    which yields

    ```
        trunc_chat_id     |        messagetype        | messagesize |   sentat
    ----------------------+---------------------------+-------------+------------
     contact-discovery-13 | MEMBERSHIP_UPDATE_MESSAGE |       15769 | 1677850449
     contact-discovery-13 | MEMBERSHIP_UPDATE_MESSAGE |       15502 | 1677850436
     0x045e098c95c9639719 | MEMBERSHIP_UPDATE_MESSAGE |       45282 | 1677850426
     contact-discovery-85 | MEMBERSHIP_UPDATE_MESSAGE |       45320 | 1677850397
     0x0413cd82a2df9c6c4b | MEMBERSHIP_UPDATE_MESSAGE |       22780 | 1677850357
     contact-discovery-13 | MEMBERSHIP_UPDATE_MESSAGE |       67856 | 1677850357
     contact-discovery-38 | MEMBERSHIP_UPDATE_MESSAGE |       67856 | 1677850357
     0x0413cd82a2df9c6c4b | MEMBERSHIP_UPDATE_MESSAGE |       22780 | 1677850356
     contact-discovery-85 | MEMBERSHIP_UPDATE_MESSAGE |       45320 | 1677850356
     contact-discovery-48 | MEMBERSHIP_UPDATE_MESSAGE |       67856 | 1677850356
    ```
    
2. Average `MEMBERSHIP_UPDATE_MESSAGE` size -

    ```sql
    SELECT
      AVG(messagesize)
    FROM
      receivedmessages
    WHERE
      messagetype = 'MEMBERSHIP_UPDATE_MESSAGE' 
      AND messagesize > 0;
    ```

    which yields

    ```
            avg
    --------------------
     67937.954692116552
    ```

    ~ 67kb
    
3. Irrelevant to check broadcast frequency, it is ad-hoc afaik

4. Number of `MEMBERSHIP_UPDATE_MESSAGE` in 1 day -

    ```sql
    SELECT 
      COUNT(*) 
    FROM 
      receivedmessages 
    WHERE 
      messagetype = 'MEMBERSHIP_UPDATE_MESSAGE' 
      AND messagesize > 0 
      AND sentat BETWEEN 1677850670 
      AND 1677764270;
    ```

    which yields

    ```
     count
    -------
      2577
    ```

    which itself is 2577 * 67kb = 172mb. Can someone from the Status team explain this message type's usage?

### 2. `COMMUNITY_DESCRIPTION`

1. Window of `COMMUNITY_DESCRIPTION` sizes sent by different communities -

    ```sql=
    SELECT 
      left(chatid, 20) as trunc_chat_id, 
      messagetype, 
      messagesize, 
      sentat 
    FROM 
      receivedmessages 
    WHERE 
      messagetype = 'COMMUNITY_DESCRIPTION' 
      AND messagesize >= 70000 
    ORDER BY sentat DESC -- to get the latest sizes
    LIMIT 
      10;
    ```
    which yields
    ```
        trunc_chat_id     |      messagetype      | messagesize |   sentat
    ----------------------+-----------------------+-------------+------------
     0x0269b18891d3b42ebd | COMMUNITY_DESCRIPTION |       87531 | 1676369752
     0x0269b18891d3b42ebd | COMMUNITY_DESCRIPTION |       87531 | 1676442625
     0x03c6552a70bc9d9407 | COMMUNITY_DESCRIPTION |      247176 | 1676317357
     0x03c6552a70bc9d9407 | COMMUNITY_DESCRIPTION |      247176 | 1676313458
     0x03c6552a70bc9d9407 | COMMUNITY_DESCRIPTION |      247176 | 1676325158
     0x03c6552a70bc9d9407 | COMMUNITY_DESCRIPTION |      247176 | 1676321259
     0x03c6552a70bc9d9407 | COMMUNITY_DESCRIPTION |      247176 | 1676309857
     0x03dcc6838078722b8c | COMMUNITY_DESCRIPTION |      318522 | 1676495674
     0x0269b18891d3b42ebd | COMMUNITY_DESCRIPTION |       87531 | 1676446526
     0x03c6552a70bc9d9407 | COMMUNITY_DESCRIPTION |      247176 | 1676496141
    ```
    

2. Average `COMMUNITY_DESCRIPTION` size sent by the Status Community

    ```sql=
    SELECT
      AVG(messagesize)
    FROM
      receivedmessages
    WHERE
      messagetype = 'COMMUNITY_DESCRIPTION'
      AND messagesize >= 70000 AND chatid = '0x03073514d4c14a7d10ae9fc9b0f05abc904d84166a6ac80add58bf6a3542a4e50a';
    ```
    > note: the chatid can be derived using `{"method":"wakuext_joinedCommunities"}` in the node management tab. Thanks @rramos.eth!
    
    which yields
    ```
             avg
    ---------------------
     346049.563314711359
    ```
    ~346kb, which is off the [estimation](https://hackmd.io/Dru3ULQSS2-II2WwkWcosg?both) by 200kb. This leads to a worse scenario in which ~401 members will lead to 1mb message size.

3. Median time difference between each broadcast of `COMMUNITY_DESCRIPTION` -

    ```sql
    SELECT 
      PERCENTILE_CONT(0.5) WITHIN GROUP(
        ORDER BY 
          diff
      ) as median 
    FROM 
      (
        SELECT 
          sentat, 
          sentat - lag(sentat) over (
            order by 
              sentat
          ) as diff 
        FROM 
          receivedmessages 
        WHERE 
          chatid = '0x03073514d4c14a7d10ae9fc9b0f05abc904d84166a6ac80add58bf6a3542a4e50a' 
          AND messagetype = 'COMMUNITY_DESCRIPTION' 
        ORDER BY 
          sentat DESC 
        LIMIT 
          1000
      ) q 
    WHERE 
      diff > 0;
    ```
    
    which yields 
    ```
     median
    --------
      3632
    ```
    The message is broadcasted ~ every hour
    
### 3. `BACKUP`

1. Window of `BACKUP` sizes sent by different peers

    ```sql
    SELECT
      left(chatid, 20) as trunc_chat_id,
      messagetype,
      messagesize,
      sentat
    FROM
      receivedmessages
    WHERE
      messagetype = 'BACKUP'
      AND messagesize > 0 ORDER BY sentat desc
    LIMIT
      10;
    ```
    which yields

    ```
        trunc_chat_id     | messagetype | messagesize |   sentat
    ----------------------+-------------+-------------+------------
     contact-discovery-04 | BACKUP      |        2264 | 1677848347
     contact-discovery-04 | BACKUP      |         608 | 1677848347
     contact-discovery-04 | BACKUP      |         119 | 1677848347
     contact-discovery-04 | BACKUP      |         738 | 1677848347
     contact-discovery-04 | BACKUP      |         109 | 1677848347
     contact-discovery-04 | BACKUP      |         109 | 1677848347
     contact-discovery-04 | BACKUP      |         109 | 1677848347
     contact-discovery-04 | BACKUP      |         112 | 1677848347
     contact-discovery-04 | BACKUP      |         612 | 1677848347
     contact-discovery-04 | BACKUP      |        9391 | 1677848347
    ```



2. Average size of the `BACKUP` message -

    ```sql=
    SELECT 
      messagetype, 
      AVG(messagesize) 
    FROM 
      (
        SELECT 
          messagetype, 
          messagesize 
        FROM 
          receivedmessages 
        WHERE 
          messagetype = 'BACKUP' 
          AND messagesize > 0 --> desktop clients using non-0.10.0rc set this to 0
        LIMIT 
          1000
      ) AS subq 
    GROUP BY 
      messagetype;
    ```
    which yields
    ```
     messagetype |        avg
    -------------+--------------------
     BACKUP      | 33749.652000000000
    ```
    ~ 33kb. With this result, the backup protocol seems fine for now without ipfs pinning/another backup mechanism. However, as the number of communities each person belongs to, this will not scale, and will require changes.

3. Average time difference between each broadcast of `BACKUP`

    > note: this query makes use of a random chatid being used to backup. Should probably be refactored

    > note: for some reason, some backup messages are being broadcasted with very low intervals to the same receiverkeyuid. Assumed due to different types of backup messages being sent. 

    ```sql
    SELECT
          AVG(diff)
        FROM
          (
            SELECT
              sentat,
              sentat - lag(sentat) over (
                order by
                  sentat
              ) as diff
            FROM
              receivedmessages
            WHERE
              chatid = 'contact-discovery-04f2e7b3394dcbf0d03bba954dbedc0bd561951a8c31a00320c6b99a40145e655da6e689d19ced6a314c6dde31ce96feb0166f19472c2a1edbeeb939e3282bce14'
              AND messagetype = 'BACKUP' AND receiverkeyuid='0x45ab6ed9f2461720737f0f3095a40d3b0c2475fe5ea4c57bc8b9fe293afcffbc'
            ORDER BY
              sentat DESC
            LIMIT
              1000000
          ) q
        WHERE
          diff > 0;
    ```

    which yields

    ```
              avg
    -----------------------
     9205.8181818181818182
    ```
    ~ 2.5 hours
    
### 4. `COMMUNITY_REQUEST_TO_JOIN_RESPONSE`

1. Window of `COMMUNITY_REQUEST_TO_JOIN_RESPONSE` sizes -

    ```sql
    SELECT
      left(chatid, 20) as trunc_chat_id,
      messagetype,
      messagesize,
      sentat
    FROM
      receivedmessages
    WHERE
      messagetype = 'COMMUNITY_REQUEST_TO_JOIN_RESPONSE'
      AND messagesize > 0 ORDER BY sentat desc
    LIMIT
      10;
    ```
    which yields

    ```
        trunc_chat_id     |            messagetype             | messagesize |   sentat
    ----------------------+------------------------------------+-------------+------------
     contact-discovery-35 | COMMUNITY_REQUEST_TO_JOIN_RESPONSE |         828 | 1677185497
     contact-discovery-28 | COMMUNITY_REQUEST_TO_JOIN_RESPONSE |       36636 | 1677145598
     contact-discovery-35 | COMMUNITY_REQUEST_TO_JOIN_RESPONSE |        1281 | 1676917897
     contact-discovery-38 | COMMUNITY_REQUEST_TO_JOIN_RESPONSE |         677 | 1676892082
     contact-discovery-38 | COMMUNITY_REQUEST_TO_JOIN_RESPONSE |         958 | 1676887818
     contact-discovery-38 | COMMUNITY_REQUEST_TO_JOIN_RESPONSE |         677 | 1676887804
     contact-discovery-28 | COMMUNITY_REQUEST_TO_JOIN_RESPONSE |       36636 | 1676877610
     contact-discovery-45 | COMMUNITY_REQUEST_TO_JOIN_RESPONSE |        1110 | 1676877544
     contact-discovery-28 | COMMUNITY_REQUEST_TO_JOIN_RESPONSE |       36496 | 1676655568
     contact-discovery-45 | COMMUNITY_REQUEST_TO_JOIN_RESPONSE |        1110 | 1676655552
    ```

2. Average size of the `COMMUNITY_REQUEST_TO_JOIN_RESPONSE` message -

    ```sql
    SELECT 
          messagetype, 
          AVG(messagesize) 
        FROM 
          (
            SELECT 
              messagetype, 
              messagesize 
            FROM 
              receivedmessages 
            WHERE 
              messagetype = 'COMMUNITY_REQUEST_TO_JOIN_RESPONSE' 
              AND messagesize > 0 --> desktop clients using non-0.10.0rc set this to 0
            LIMIT 
              1000
          ) AS subq 
        GROUP BY 
          messagetype;
    ```

    which yields

    ```
                messagetype             |        avg
    ------------------------------------+--------------------
     COMMUNITY_REQUEST_TO_JOIN_RESPONSE | 24488.108695652174
    ```
    ~24kb
    
3. Irrelevant to check broadcast frequency, it is ad-hoc afaik

4. Number of `COMMUNITY_REQUEST_TO_JOIN_RESPONSE` in 1 day -

    ```sql
    SELECT 
      COUNT(*) 
    FROM 
      receivedmessages 
    WHERE 
      messagetype = 'COMMUNITY_REQUEST_TO_JOIN_RESPONSE' 
      AND messagesize > 0 
      AND sentat BETWEEN 1677099097
      AND 1677185497;
    ```

    which yields

    ```
     count
    -------
      2
    ```


### 5. `SYNC_PROFILE_PICTURE`

1. Window of `SYNC_PROFILE_PICTURE` sizes -

    ```sql
    SELECT
      left(chatid, 20) as trunc_chat_id,
      messagetype,
      messagesize,
      sentat
    FROM
      receivedmessages
    WHERE
      messagetype = 'SYNC_PROFILE_PICTURE'
      AND messagesize > 0 ORDER BY sentat desc
    LIMIT
      10;
    ```
    which yields
    ```
            trunc_chat_id     |     messagetype      | messagesize |   sentat
    ----------------------+----------------------+-------------+------------
     0x0461f576da67dc0bca | SYNC_PROFILE_PICTURE |       19550 | 1677162569
     0x0461f576da67dc0bca | SYNC_PROFILE_PICTURE |       19550 | 1677162327
     0x0461f576da67dc0bca | SYNC_PROFILE_PICTURE |       19550 | 1677162099
     0x0461f576da67dc0bca | SYNC_PROFILE_PICTURE |       19550 | 1677162038
     0x0461f576da67dc0bca | SYNC_PROFILE_PICTURE |       19550 | 1677162005
    ```
    
2. Average size of the `SYNC_PROFILE_PICTURE` message -

    ```sql
    SELECT 
        messagetype, 
        AVG(messagesize) 
            FROM 
              (
                SELECT 
                  messagetype, 
                  messagesize 
                FROM 
                  receivedmessages 
                WHERE 
                  messagetype = 'SYNC_PROFILE_PICTURE' 
                  AND messagesize > 0 --> desktop clients using non-0.10.0rc set this to 0
                LIMIT 
                  1000
              ) AS subq 
            GROUP BY 
              messagetype;
    ```
    which yields
    ```
         messagetype      |        avg
    ----------------------+--------------------
     SYNC_PROFILE_PICTURE | 19550.000000000000
     ```
     
3. Irrelevant to check broadcast frequency, it is ad-hoc afaik

4. Number of `SYNC_PROFILE_PICTURE` in 1 day -

    ```sql
    SELECT 
      COUNT(*) 
    FROM 
      receivedmessages 
    WHERE 
      messagetype = 'SYNC_PROFILE_PICTURE' 
      AND messagesize > 0 
      AND sentat BETWEEN 1677076169
      AND 1677162569;
    ```

    which yields
    ```
     count
    -------
         5
    ```
    
## Conclusion

It is assumed that `UNKNOWN` messages originate from 1:1 chats.

The major bandwidth usage comes from `MEMBERSHIP_UPDATE_MESSAGE` and `COMMUNITY_DESCRIPTION`

It is recommended to optimize the payloads to ensure that scaling problems are solved.