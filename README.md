# Enterprise Hybrid Sandbox: Großkonzern-Infrastruktur auf einem einzigen Laptop simulieren

Dieses Projekt beweist, dass man kein riesiges Server-Rechenzentrum braucht, um komplexe Firmen-Infrastrukturen zu verstehen. Mit nur einem ganz normalen, privaten Laptop und der Hilfe von virtuellen Maschinen (VMs) habe ich ein hybrides, plattformübergreifendes Netzwerk aufgebaut.

Das gesamte System ist so designt, dass es eins zu eins auf das Niveau von Großkonzernen (Enterprise-Level) skaliert werden könnte, ohne dass man die Logik dahinter ändern muss. Es schließt die Lücke zwischen der Windows-Welt (Zentrale Verwaltung) und der Linux-Welt (Abgesicherte Server).

🏗️ Kernprozesse & Technologiesäulen
Um ein realistisches Firmennetzwerk abzubilden, wurden vier tragende Säulen kombiniert:

 -Zentrales Identitätsmanagement (Active Directory / AD DS): Ein Windows Server 2022 wurde als „Gehirn“ des Netzwerks eingerichtet. Dieses System verwaltet alle Rechte zentral und ist so skalierbar, dass morgen problemlos 50.000 Mitarbeiter hinzugefügt werden könnten.

 -Plattformübergreifender DNS-Dienst: Der Windows-Server fungiert als die einzige Wahrheit im Netz (Single Source of Truth). Er übersetzt Namen in IPs für alle Systeme.

 -Abgesicherte Netzwerk-Segmentierung: Über ein komplett isoliertes internes Netzwerk (intnet) wurden die Server vom Internet abgeschottet, um Angriffe von außen unmöglich zu machen.

Linux-Enterprise-Integration: Ein gehärteter Ubuntu-Linux-Server wurde so konfiguriert, dass er der Windows-Infrastruktur vertraut und seine Namensauflösung über sie abwickelt.

🎨 Architektur-Hintergedanken & Design-Verständnis
  Warum haben wir das so gebaut? Im echten Berufsleben nutzt kaum ein Großkonzern nur ein einziges Betriebssystem. Die Verwaltung (Mitarbeiter, PCs, Rechte) läuft fast immer über Windows, während Webseiten, Datenbanken oder interne Anwendungen auf stabilen Linux-Servern laufen.

  Der Clou hierbei ist das "Headless-Design": Sowohl der Windows Server (als Core-Version) als auch das Linux-System laufen komplett ohne grafische Oberfläche (GUI). Als Administrator bediene ich alles rein über Textbefehle (CLI). Das spart massiv Hardware-Ressourcen auf dem Laptop und schließt Sicherheitslücken, die durch grafische Programme entstehen würden.

📸 Projekt-Meilensteine & Nachweise
Der Erfolg des Setups wird durch die drei wichtigsten Kern-Ergebnisse dokumentiert:

1. Das Verzeichnisnetzwerk steht
 -Der Windows Domain Controller läuft fehlerfrei unter der Domäne firma.local.

Nachweis: <img width="1466" height="1816" alt="Screenshot from 2026-06-19 17-38-18" src="https://github.com/user-attachments/assets/21d56525-1680-43f6-b530-e68a29323d6e" />

Linux ist angebunden
 -Der Ubuntu-Server hat die Schnittstellen-Konfiguration geladen und nutzt offiziell die Windows-VM als DNS-Zentrale.

Nachweis: <img width="1403" height="442" alt="Screenshot from 2026-06-19 18-07-09" src="https://github.com/user-attachments/assets/9901b166-3538-4fdc-a1dd-8f62bce95471" />


3. Der finale Brückenschlag
 -Linux findet den Windows-Server über den Namen im Netzwerk. Die Verbindung steht.

Nachweis:<img width="1464" height="1816" alt="Screenshot from 2026-06-19 17-49-05" src="https://github.com/user-attachments/assets/e01f2ed4-23aa-48cc-bd76-0c2b18a16b3b" />


🛠️ Deep Dive: Das Troubleshooting-Rabbit-Hole & Das 4-Augen-Prinzip
  Der härteste Brocken in diesem Projekt waren keine komplexen Server-Theorien, sondern eine Serie von unnötigen Fehlstunden, ausgelöst durch einen einzigen, winzigen IP-Tippfehler.

  Was war passiert?
Während der Einrichtung des Netzwerks wurde der Windows-Server versehentlich auf die IP-Adresse 198.168.56.10 konfiguriert – richtig gewesen wäre das private Subnetz 192.168.56.10. Ein einziger Zahlendreher (eine 8 statt einer 2).

Die fatalen Folgen des "Rabbit-Holes":
Weil dieser Tippfehler so unscheinbar war, übersah ich ihn bei den ersten Kontrollen. Das System verhielt sich plötzlich völlig unvorhersehbar: Die Active-Directory-Installation brach mit roten Fehlermeldungen ab, und die Linux-VM konnte den Server nicht erreichen.

Ich rutschte tief in ein Fehlersuch-Suchtfeld (Rabbit-Hole) ab: Ich suchte nach defekten Windows-Diensten, prüfte virtuelle Netzwerk-Switches und änderte komplexe DNS-Konfigurationen – alles völlig umsonst, weil das Fundament schief war.

Die wichtigsten Learnings für meine Umschulung:
 -Betriebsblindheit ist real: Wenn man stundenlang auf die eigenen Befehle starrt, liest das Gehirn nur noch das, was dort stehen sollte. Man wird blind für die Realität.

 -Die Wichtigkeit des 4-Augen-Prinzips: In der IT ist es keine Schande, Hilfe zu holen. Erst der Blick von außen (eine zweite Person oder ein frisches Auge) bricht diese Blindheit. Was man selbst zehnmal überliest, sieht der andere in fünf Sekunden.

 -Current-Info-Vergleich statt Raten: Anstatt bei Fehlern wild neue Befehle auszuprobieren (was alles nur noch schlimmer macht), muss man radikal einen Schritt zurückgehen, alle aktuellen Ist-Daten (ipconfig, SConfig) auslesen und stur mit dem Soll-Plan abgleichen.

 Nachdem der Zahlendreher entdeckt und korrigiert wurde, lief das gesamte Großkonzern-Setup innerhalb von Sekunden fehlerfrei durch.

🏁 Fazit: Warum dieses Projekt als mein nächster Step Sinn gemacht hat
Nachdem ich in Projekt 1 die Grundlagen von einzelnen Betriebssystemen gelernt habe, war Projekt 2 der absolut logische und notwendige nächste Schritt für meine Entwicklung zum Fachinformatiker.

In der echten Arbeitswelt arbeitet kein Server alleine. Ich habe hier bewiesen, dass ich nicht nur einzelne Systeme bedienen, sondern heterogene Netzwerke (Windows + Linux) im Verbund denken, aufbauen und verknüpfen kann.

Ich habe gelernt, mich durch zähe Fehlersuchen durchzubeißen, CLI-basiert zu arbeiten und Systeme so zu planen, dass sie theoretisch sofort in einem echten Unternehmen mit tausenden Rechnern eingesetzt werden könnten. Für meine Umschulung und meinen zukünftigen Einstieg in die Praxis war dieses Projekt der absolute Meilenstein.
