Zur effizienten Unterstützung eines agilen und verteilten Entwicklungsprozess sollten Sie die nötige Infrastruktur an allen Stellen automatisieren. Jedes manuelle Eingreifen benötigt spezielle Kenntnisse und Fähigkeiten die nicht immer zur Verfügung stehen könnten. Ziel muss das Ausliefern beliebiger Komponenten auf Knopfdruck ohne besondere Kenntnisse sein. Um Entwicklern und Operations völlige technologische Freiheit zu ermöglichen, hat sich die Verwendung  von VM Containern als besonders flexibel erwiesen. Dabei kann jede Anwendungskomponente automatisiert in beliebig vielen unterschiedlichen Version mittels HTTP API jederzeit veröffentlicht, im Service-Discovery registriert und deregistriet und in das Instrumentations- und Monitoring System ausgewertet werden. So ergeben sich 3 einfache Schritte für die Veröffentlichung.

* Code und Dokumentation
* Version, Build und Publish
* Konfiguration und Deployment

In unserer Praxis haben sich für uns folgende Best-Practices  in der Umsetzung von Microservices in eine automatisierte Infrastruktur ergeben:

* Log to StdOut
* Konfiguration via Env-Var
* Graceful-Shutdown
* Request Rate Limits
* Verwendung temporärer Zustandsspeicher (z.B. In-Memory Cache)
* Health-Check als HTTP Endpunkt