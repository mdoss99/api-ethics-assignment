\# Task 1 — Classify and Handle PII Fields



| Field           | Type           | Action      | Reason                                              |

| --------------- | -------------- | ----------- | --------------------------------------------------- |

| full\_name       | Direct PII     | Drop        | Directly identifies a person                        |

| email           | Direct PII     | Mask        | Can identify a person, masking protects privacy     |

| date\_of\_birth   | Indirect PII   | Generalize  | Can identify a person when combined with other data |

| zip\_code        | Indirect PII   | Generalize  | Reveals location, can lead to identification        |

| job\_title       | Indirect PII   | Keep        | Not enough to identify a person alone               |

| diagnosis\_notes | Sensitive Data | Remove/Mask | May contain personal or health-related information  |



\---



\# Task 2 — Audit API Script



\## Issue 1: Hardcoded API Key



Problem:

The API key is written directly in the code. This is a security risk because anyone can access and misuse it.



Fix:



```python

import os

API\_KEY = os.getenv("API\_KEY")

```



\---



\## Issue 2: No Data Privacy / Consent Handling



Problem:

The script collects and stores personal data without checking user consent. This is not ethically or legally safe.



Fix:



```python

if user\_has\_consented:

&#x20;   save\_to\_database(records)

```



\---



\## Corrected Full Code



```python

import requests

import os



API\_URL = "https://healthstats-api.example.com/records"

API\_KEY = os.getenv("API\_KEY")



records = \[]



for page in range(1, 101):

&#x20;   response = requests.get(API\_URL, params={"page": page, "key": API\_KEY})

&#x20;   data = response.json()



&#x20;   # Remove direct PII before storing

&#x20;   for record in data\["results"]:

&#x20;       record.pop("full\_name", None)

&#x20;       record.pop("email", None)



&#x20;   records.extend(data\["results"])



\# Store data only if user consent is given

if user\_has\_consented:

&#x20;   save\_to\_database(records)

```



