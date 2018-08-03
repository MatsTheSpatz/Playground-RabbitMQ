# ngrok 

**Instantly create a public HTTP(S) URL for a web site running locally on your development machine.**

https://ngrok.com/


---
#### How to start
- Install on Windows (download the ngrok client, a single binary with zero run-time dependencies.) (--> see c:/tools/)
- Use **````ngrok````** from the command-line ("ngrok.exe") 
  - Check ````ngrok --help```` 
  - Proivde authtoken (if used for first time): ````ngrok authtoken {...}````
  - Start with: ````ngrok http 8080 -host-header="localhost:8080````
    - assuming local page exposed on 8080
    - ````-host-header````-option needed, otherwise I get "Bad Request" (??)
    - once started --> public URL is displayed

---
#### How to use
Once started
- public URL displayed in shell (ngrok)
- check ngrok shell for state
- use local inspector: http://localhost:4040/inspect/http

---
#### Credentials
- *work-pwd*
- *standard-unsafe-pwd*
