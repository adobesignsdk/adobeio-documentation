# Check the Status

Adobe Sign can return the current status of the agreement and a complete history of events that have happened on that particular agreement. The simplest mechanism is for your application to provide a webhook URL when sending the document for signature. Adobe Sign will then ping your webhook with the appropriate [webhook event](../webhooks/webhook_events.md) whenever the status of the agreement changes.

![Checking the status of an agreement](../img/sign_devguide_2.png)

You can also get the most current status of an agreement by sending a GET request to `/agreements/{agreementid}`:

```http
GET /api/rest/v6/agreements/3AAABLblqZNOTREALAGREEMENTID5_BjiH HTTP/1.1
Host: api.na1.echosign.com
Authorization: Bearer 3AAANOTREALTOKENMS-4ATH
```

You need to provide your access token in the `Authorization` header and the `agreementId` in the API call itself.

You will get the following JSON response:

```json
{
  "events":[
    {
      "actingUserEmail":"someone@somecomp.com",
      "actingUserIpAddress":"103.43.112.98",
      "date":"2016-05-11T00:30:32-07:00",
      "description":"Document created by Frank Jennings",
      "participantEmail":"someone@somecomp.com",
      "type":"CREATED",
      "versionId":"3AAANOTREALIDsb_IZ"
    },
    {
      "actingUserEmail":"someone@somemail.com",
      "date":"2016-05-11T00:30:42-07:00",
      "description":"Sent out for signature to Phantom Jennings",
      "participantEmail":"someid@somecompany.com",
      "type":"SIGNATURE_REQUESTED"
    }
  ],
  "latestVersionId":"3AAABLbNOTREALID1UIGbvj",
  "locale":"en_US",
  "name":"[DEMO USE ONLY] MyTestAgreement",
  "participantSetInfos":[
    {
      "participantSetMemberInfos":[
        {
          "company":"Adobe Systems Inc. (CCE & DC)",
          "email":"someid@somecompany.com",
          "name":"Phantom Jennings"
        }
      ],
      "roles":[
        "SIGNER"
      ],
      "signingOrder":1,
      "status":"WAITING_FOR_MY_SIGNATURE"
    },
    {
      "participantSetMemberInfos":[
        {
          "company":"Adobe",
          "email":"someid@somecompany.com",
          "name":"Frank Jennings",
          "title":"Approver"
        }
      ],
      "roles":[
        "SENDER"
      ],
      "status":"OUT_FOR_SIGNATURE"
    }
  ],
  "status":"OUT_FOR_SIGNATURE",
  "agreementId":"3AAABNOTREALIDI5_BjiH",
  "modifiable":false,
  "nextParticipantSetInfos":[
    {
      "nextParticipantSetMemberInfos":[
        {
          "email":"somemail@somecompany.com",
          "name":"Phantom Jennings",
          "waitingSince":"2016-05-11T00:30:32-07:00"
        }
      ]
    }
  ],
  "vaultingEnabled":false
}
```

By default, the callback URL is called whenever an event involving a particular transaction occurs in Adobe Sign. The callback includes the ID of the agreement whose status has changed, the current status of the agreement, and information on the event that resulted in the callback. Your application logic can evaluate the received status and decide whether to perform an action in the calling system.

In addition to HTTPS GET, Adobe Sign also alternatively supports PUT for receiving events about the signature process. Included in the request will be the completed signed PDF. Adobe Sign uses an PUT request to return the signed PDF. Please ensure that your application can correctly handle such a request. Please contact Adobe Support or your assigned Client Success Manager to get your account configured to receive PUT events.

The second mechanism to reflect the most current or up-to-date status of an agreement sent for signature is for your application to periodically poll Adobe Sign regarding the agreement&rsquo;s status. The upside of polling is that it can be used in cases where your calling application is behind your firewall and not accessible from the Internet, thus enabling Adobe Sign to complete a callback. The down side of polling is that you have to create a scheduling mechanism within your application to periodically query the status of all documents that were not yet signed, check whether the document&rsquo;s status has changed, and update your system accordingly. If you choose to use polling, we recommend you have different policies based on document "age". In other words, you would reduce the frequency of polling for documents not signed after a certain number of days.

[TRY IT OUT](https://secure.na1.echosign.com/public/docs/restapi/v6#!/agreements/_0_1_2)