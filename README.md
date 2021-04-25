# DigiTecten

digital + architects = DigiTecten


Unser Projekt zum SDaCathon 2021 (Visuelle Challenge)

## Idee

Wir wollen Symbole auf Bauplänen erkennen und verschiedene Funktionen wie suchen nach / zählen von Piktogrammen erlauben.

Außerdem soll der Benutzer die Möglichkeit bekommen das Modell eigenständig zu trainieren. Hierfür kann er jeden beliebigen Bereich markieren und ein Label dafür vergeben.
Die Software generiert aus dem selektierten Bereich dynamische Testdaten, aktualisiert das Modell und führt anschließend die Evaluierung aus. 
Die Benutzer haben die Verantwortung der KI nur sinnvolle Piktogramme (Bildausschnitte) zu lernen.

Der Benutzer bekommt danach gleich Feedback durch das gelernte Model.
Da es sich hierbei um eine Webanwendung handelt, sind die zusätzlich gelernten Piktogramme für alle anderen Benutzer verfügbar. 

Visuelle Darstellung:
![Original PDF nach Zerlegung](assets/flow.png)

## Konzept

Unsere Erkennung läuft in mehreren Schritten. Zunächst werden interessante Bereiche identifiziert:

1. Der Nutzer lädt eine PDF Datei mit einem Bauplan in eine Webanwendung ([webtool_backend](https://github.com/DigiTecten/webtool_backend), [webtool](https://github.com/DigiTecten/webtool))
2. Die PDF Datei wird in eine große Grafik (PNG) und in eine SVG Vektorgrafik konvertiert (Tools vorhanden 🛍) ([pdf2svg](https://cityinthesky.co.uk/opensource/pdf2svg/), [pdf2image](https://pypi.org/project/pdf2image/))
3. Die PNG Datei wird in kleine, überlappende Segmente zerlegt und jeweils an ein zuvor trainiertes CustomVisionAI Modell gesendet ([Testcode zum Trainieren unseres Modells](https://github.com/DigiTecten/custom-vision-tests))
   ![Original PDF nach Zerlegung](assets/uploaded_part.png)
4. Der Nutzer bekommt eine grobe Erkennung der interessanten Elemente in der PDF Datei angezeigt (Das sieht schon ganz gut aus 🚀)
   ![Interessante Bereiche mit ML identifiziert](assets/relevant_areas.png)
5. Die gefunden Bereiche werden großflächig im SVG "ausgeschnitten" und für den nächsten Schritt "zurückgelegt" (Haben wir noch nicht 😕)

Die interessanten Bereiche sollen nun noch genauer klassifiziert werden. In unseren Tests hatten wir keine guten Ergebnisse mit Machinelearning für die Detailklassifikation. Die Elemente sind sich teilweise zu ähnlich.
Da wir nun den Suchbereich stark eingeschränkt haben, haben wir die Möglichkeit die vorhanden Vektorgrafiken zu vergleichen: [Idee (svg-diff)](https://github.com/DigiTecten/svg-diff)

Die Eingabe des svg-diff Algorithmus werden vom Nutzer in der Legende ausgewählt und vom neuronalen Netz vorgegeben (interessante Bereiche).
