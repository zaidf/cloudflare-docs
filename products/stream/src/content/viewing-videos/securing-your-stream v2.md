---
order: 3
pcx-content-type: how-to
---

# Securing your Stream

## Signed URLs / Tokens

By default, videos on  Stream can be viewed by anyone at anytime if they have the video id. If you want to make your video private by default and only allow certain users access, you would use the signed URL feature. When you mark a video to require signed URL, it can no longer be accessed publicly with only the video id. Instead, the user will need a signed url token to watch or download the video.

Here are some common use cases for using signed URLs:

* Restricting access so only logged in members can watch a particular video
* Let users watch your video for a limited time period (ie. 24 hours)
* Restricting access based on geolocation 

## Two Ways to Generate Signed Tokens

You can program your app to generate token in two ways:

### By usingse the /token endpoint 
The simplest way to create a signed url token is by calling the /token endpoint. This is recommended for testing purposes or if you are generating less than 10,000 tokens per day.

### By using an open-source library to generate tokens
If you have tens of thousands of daily users and need to generate a high-volume of tokens without calling the /token endpoint *each time*, you can create a key *once* and create tokens. This way, you do not need to call a Stream endpoint every time you need to generate a token.

## Option 1: Using the /token endpoint

You can call the /token endpoint for any video that is marked private to get a signed url token which expires in one hour:

```bash
curl \
-H "Authorization: Bearer $TOKEN" \
https://api.cloudflare.com/client/v4/accounts/$ACCOUNT/stream/$VIDEOID/token
```
```json
{
  "result": {
    "token": "eyJhbGciOiJSUzI1NiIsImtpZCI6ImNkYzkzNTk4MmY4MDc1ZjJlZjk2MTA2ZDg1ZmNkODM4In0.eyJraWQiOiJjZGM5MzU5ODJmODA3NWYyZWY5NjEwNmQ4NWZjZDgzOCIsImV4cCI6IjE2MjE4ODk2NTciLCJuYmYiOiIxNjIxODgyNDU3In0.iHGMvwOh2-SuqUG7kp2GeLXyKvMavP-I2rYCni9odNwms7imW429bM2tKs3G9INms8gSc7fzm8hNEYWOhGHWRBaaCs3U9H4DRWaFOvn0sJWLBitGuF_YaZM5O6fqJPTAwhgFKdikyk9zVzHrIJ0PfBL0NsTgwDxLkJjEAEULQJpiQU1DNm0w5ctasdbw77YtDwdZ01g924Dm6jIsWolW0Ic0AevCLyVdg501Ki9hSF7kYST0egcll47jmoMMni7ujQCJI1XEAOas32DdjnMvU8vXrYbaHk1m1oXlm319rDYghOHed9kr293KM7ivtZNlhYceSzOpyAmqNFS7mearyQ"
  },
  "success": true,
  "errors": [],
  "messages": []
}
```

To use the signed URL token returned in the response, replace the video id in the embed code with the token:

```HTML

<iframe src="https://iframe.videodelivery.net/eyJhbGciOiJSUzI1NiIsImtpZCI6ImNkYzkzNTk4MmY4MDc1ZjJlZjk2MTA2ZDg1ZmNkODM4In0.eyJraWQiOiJjZGM5MzU5ODJmODA3NWYyZWY5NjEwNmQ4NWZjZDgzOCIsImV4cCI6IjE2MjE4ODk2NTciLCJuYmYiOiIxNjIxODgyNDU3In0.iHGMvwOh2-SuqUG7kp2GeLXyKvMavP-I2rYCni9odNwms7imW429bM2tKs3G9INms8gSc7fzm8hNEYWOhGHWRBaaCs3U9H4DRWaFOvn0sJWLBitGuF_YaZM5O6fqJPTAwhgFKdikyk9zVzHrIJ0PfBL0NsTgwDxLkJjEAEULQJpiQU1DNm0w5ctasdbw77YtDwdZ01g924Dm6jIsWolW0Ic0AevCLyVdg501Ki9hSF7kYST0egcll47jmoMMni7ujQCJI1XEAOas32DdjnMvU8vXrYbaHk1m1oXlm319rDYghOHed9kr293KM7ivtZNlhYceSzOpyAmqNFS7mearyQ" style="border: none;" height="720" width="1280" allow="accelerometer; gyroscope; autoplay; encrypted-media; picture-in-picture;" allowfullscreen="true"></iframe>

```
