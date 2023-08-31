

**Custom Resource**
flightticket.yml
```yml
apiVersion: flights.com/v1
kind: FlightTicket
metadata:
    name: my-flight-ticket
spec:
    from: <origin>
    to: <destination>
    number: 2
```

flightticket-custom-definition.yml
```yml
apiVersion: apiextensions.k8s.io/v1
kind: CustomResourceDefinition
metadata:
    name: flighttickets.flights.com
spec:
    scope: Namespaced
    group: flights.com
    names:
        kind: FlightTicket
        singular: flightticket
        plural: flighttickets
        shortNames:
            - ft
    versions:
        - name: v1
          served: true
          storage: true
          schema:
            openAPIV3Schema:
                type: object
                properties:
                    spec:
                        type: object
                        properties:
                            from:
                                type: string
                            to:
                                type: string
                            number:
                                type: integer  
                                minimum: <1>
                                maximum: <10>            
``

`kubectl create -f flightticket-custom-definition.yml`
`kubectl create -f flightticket.yml`
`kubectl api-resources`

## Build a custom controller to handle custom resources