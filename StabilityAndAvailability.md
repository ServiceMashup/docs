## Stabilität und Verfügbarkeit

In traditionellen serviceorientierten Architekturen wird die Kommunikation, sowohl zwischen Frontend und Backend als auch zwischen verschieden Diensten, meist über in Konfigurationsdateien hinterlegten Adressen realisiert. Dieses enge Koppelung führt in der Praxis häufig zu Problemen bei der Weiterentwicklung der Systeme, z.B. wenn eine neue Version eines Dienstes veröffentlicht werden soll, welche nicht mehr abwärtskompatibel ist, während es jedoch noch weitere Dienste im System gibt, die von ersterem abhängen. Ebenso kann hier eine horizontale Skalierung nur durch vorgeschaltete Load Balancer oder direkt auf DNS-Ebene realisiert werden, was wiederum die Komplexität der zu pflegenden Infrastruktur erhöht. Verzichtet man auf solche Maßnahmen, setzt man gleichzeitig die Verfügbarkeit des Gesamtsystems aufs Spiel: In einem Netz aus voneinander abhängigen Diensten dürfte dann kein einziger dieser Dienste ausfallen.
Microservices verfolgen von ihrem Wesen her einen anderen Ansatz: Zum einen sind Dienste im Optimalfall völlig autark und haben so wenig wie möglich (im besten Fall keine) externe Abhängigkeiten, zum anderen sind sie derart gestaltet, dass sie selbst bei einem Ausfall dieser Abhängigkeiten völlig oder zumindest nur leicht eingeschränkt operabel bleiben. Ist der Partner-Dienst dann zu einem späteren Zeitpunkt wieder verfügbar, können noch anstehende Operationen abgeschlossen oder veraltete Daten auf den neuesten Stand gebracht werden. Daraus folgt, dass Kommunikation zwischen Diensten im Regelfall asynchron und nicht im Kontext einer Benutzerinteraktion stattfindet. Es folgt auch daraus, dass die Verfügbarkeit veralteter Daten als immer noch besser zu bewerten ist als die Nicht-Verfügbarkeit jeglicher Daten oder sogar das Auftreten eines Fehlerfalls, im Falle des eCommerce-Beispiels schon rein aus geschäftlichem Interesse: Jeder Auftrag ist kostbar, auch wenn er vielleicht momentan aufgrund eines technischen Ausfalls nicht sofort verarbeitet werden kann.
Für Microservice-Architekturen bietet sich eine Reihe von Maßnahmen zur Fehlerkompensation und Ausfallsicherheit an:

### Service Discovery und Fallbacks

Nehmen wir an, Service A möchte seinen eigenen Datenbestand aktualisieren und muss dazu Service B aufrufen. In diesem Fall ist es für Service A im Grunde irrelevant, wieviele Instanzen von Service B laufen, unter welchen Addressen sie erreichbar sind, und welche der laufenden Instanzen nun tatsächlich aufgerufen werden soll. Das einzige Interesse von Service A ist, eine HTTP-Verbindung zu einem intakten Instanz von Service B zu erlangen. Anstatt nun Service A über Konfigurationsdaten mit sämtlichen Zugangsinformationen zu Service B auszustatten, können diese Informationen auch zur Laufzeit ermittelt werden. Dazu benötigt man einen Dienst, der als Registry fungiert und Daten über sämtliche im System laufende Dienste anbietet. Von dieser Registrierungsstelle können nach Bedarf die aktuellen Adressen eines Dienstes anhand seines Namens erfragt werden. Die somit erlangte Liste von Adressen kann für eine bestimmte Zeit vorgehalten werden, bis eine erneute Anfrage stattfindet. Möchte man nun eine Verbindung herstellen, probiert man die Adressen systematisch durch. Antwortet die Gegenstelle nicht, so wird versucht, eine Verbindung mit der nächsten Adresse aus der Liste herzustellen. Diesen ersten naiven Ansatz kann man dann weiter ausbauen, z.B. durch den Einsatz eines Circuit Breakers oder des Round-Robin-Verfahrens. Eine beispielhafte Implementierung im Rahmen eines AngularJS-Services ist in Listing A1 dargestellt.

Listing A1

```js
function ajax(method, serviceName, path, value) {
  return getServiceUrls(serviceName).then(function (urls) {
    if (!urls.length) throw new Error('No endpoint configured for service ' + serviceName);

    // Map urls to a list of operations
    var funcs = urls.map(function (url) {
      return $http[method].bind($http, 'http://' + url + path, value, { timeout: config.timeout });
    });

    return invokeUntilResolved(funcs);
  });
}

function getServiceUrls(serviceName) {
  if (cache[serviceName]) return $q.when(cache[serviceName]);

  var funcs = config.discoveryServers.map(function (discoveryServer) {
    return $http.get.bind($http, discoveryServer + '/v1/catalog/service/' + serviceName);
  });

  return invokeUntilResolved(funcs).then(function (result) {
    var serviceUrls = result.data.map(function (itm) {
      return itm.Address + ':' + itm.ServicePort;
    });

    cache[serviceName] = serviceUrls;
    return serviceUrls;
  });
}

function invokeUntilResolved(funcs) {
  // Invoke functions that return promises sequentially
  // and return the first resolved promise.
  // Invoke the next function only when the return value
  // of the previous function is a rejected promise.
  return funcs.reduce(function (previous, next) {
    return previous.catch(next);
  }, $q.reject(new Error('No function specified')));
}
```

Die Funktion `ajax` holt zuerst vom Discovery-Service sämtliche verfügbaren URLs für den angegebenen Service-Namen und erzeugt daraus eine Liste von Funktionen, in denen jeweils der HTTP-Aufruf an die entsprechende URL initiiert wird. Die eigentliche Fallback-Logik besteht aus der Verkettung dieser Funktionen, wobei die nächste immer nur dann aufgerufen wird, wenn der vorherige Aufruf zu einem Fehler geführt hat. Dies lässt sich in JavaScript elegant durch die Verkettung von Promises implementieren und geschieht in der Funktion `invokeUntilResolved`. Die Funktion `getServiceUrls` funktioniert nach dem gleichen Muster, schließlich muss es auch vom Discovery-Dienst mehrere Instanzen geben. Zusätzlich werden die Antworten dieser Aufrufe noch in einem localen Cache abgelegt, um bei weiteren Anfragen mit demselben Dienstnamen die Liste nicht noch einmal anfordern zu müssen.
Das Auffinden und Auswählen von Services ist eine Querschnittsfunktionalität, die höchstwahrscheinlich unverändert in mehreren Diensten zum Einsatz kommen wird. Aus diesem Grunde bietet es sich an, diese Funktion durch ein importierbares SDK zentral für alle Teams zur Verfügung zu stellen, z.B. als NuGet-Package, NPM-Modul oder Bower-Paket.

### Statefulness & Integration (Orchestration) UI (Code)

* Atom Writes (Code)
* https://github.com/ServiceMashup/web-app/blob/master/app/cart.directive.js

### Data Replication (Code)

* Replicate data from Data Warehouse
