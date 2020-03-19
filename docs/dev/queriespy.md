# Example API Query (Python)

!!! note
    This example is available in the Jupyter notebook at: [github.com/intrahealth/client-registry/examples/simple_query.ipynb](https://github.com/intrahealth/client-registry/blob/master/examples/simple_query.ipynb)

This is a simple query in Python exactly like the cURL query:
```sh
curl --cert sampleclientcertificates/openmrs.p12 --cert-type p12 --cacert certificates/server_cert.pem -d @/Users/richard/src/github.com/openhie/client-registry/DemoData/patient1_openmrs.json -H "Content-Type: application/json" -XPOST https://localhost:3000/addPatient
```

The only requirement is a version of the requests package that has been modified to take p12 certs.
```sh
pip3 install requests_pkcs12
```

Import the required modules.
```py
from pathlib import Path
# import requests modded dor pkcs12
from requests_pkcs12 import get, post
```

This example assumes running from `github.com/intrahealth/client-registry/examples`. Change paths as required. Using the Path module ensures that the path resolution works in Mac, Linux, Windows.
```py
clientcert_folder = Path("../server/sampleclientcertificates")
clientcert = clientcert_folder / "openmrs.p12"

servercert_folder = Path("../server/certificates")
servercert = servercert_folder / "server_cert.pem"

payload_folder = Path("../DemoData/")
payload_bytes = payload_folder / "patient1_openmrs.json"
payload = open(payload_bytes)
```

Define headers and initiated the POST request.
```py
headers = {'Content-Type': 'application/json'}
response = post("https://localhost:3000/Patient", headers=headers, data=payload, 
                pkcs12_filename=clientcert, 
                pkcs12_password='', 
                verify=servercert)
print(response)
```