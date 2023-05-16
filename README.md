# Okta-inline-hook
Sample code to use for testing Okta SAML inline hook

This is some Go Lang code that can be used to test a simple
Okta Inline SAML web hook.

```
package main

import (
	"fmt"
	"log"
	"net/http"
)

func main() {

	http.HandleFunc("/", func(w http.ResponseWriter, r *http.Request) {
		myResponse := `{"commands":[{"type":"com.okta.assertion.patch","value":[{"op":"add","path":"/claims/hookAttribute","value":{"attributes":{"NameFormat":"urn:oasis:names:tc:SAML:2.0:attrname-format:basic"},"attributeValues":[{"attributes":{"xsi:type":"xs:string"},"value":"NOT_STORED_IN_OKTA"}]}}]}]}`
		w.WriteHeader(http.StatusOK)
		fmt.Println("The status code we got is:", 200)
		fmt.Fprintf(w, myResponse)
	})

	http.HandleFunc("/hi", func(w http.ResponseWriter, r *http.Request) {
		fmt.Fprintf(w, "Hi")
	})

	log.Fatal(http.ListenAndServe(":80", nil))

}
```
