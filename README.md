# K-Means Clustering: Algorithmische Implementierung und Varianzanalyse

## Projektübersicht

Dieses Repository beinhaltet eine **"From-Scratch"-Implementierung** des K-Means-Clustering-Algorithmus in Python. Das Projekt dient der Untersuchung unüberwachter Lernverfahren (Unsupervised Learning) unter Verzicht auf High-Level-Abstraktionen (wie `sklearn.cluster`).

Der Fokus liegt auf der mathematischen Nachbildung des Expectation-Maximization-Prinzips, der Implementierung einer robusten Zentroiden-Initialisierung zur Vermeidung lokaler Minima und der quantitativen Evaluierung der Modellkomplexität mittels Varianzanalyse (Elbow-Methode).

## Theoretischer Hintergrund

K-Means ist ein iteratives Verfahren zur Partitionierung eines Datensatzes $X = \{x_1, ..., x_n\}$ in $k$ disjunkte Cluster $C = \{C_1, ..., C_k\}$. Ziel ist die Minimierung der intra-cluster Varianz (Within-Cluster Sum of Squares).

Die Zielfunktion (Loss Function) $J$, die in diesem Projekt minimiert wird, ist definiert als:

$$J = \sum_{j=1}^{k} \sum_{i \in C_j} ||x_i - \mu_j||^2$$

Wobei $\mu_j$ der Schwerpunkt (Zentroid) des Clusters $C_j$ ist. Der Algorithmus konvergiert durch abwechselnde Ausführung zweier Schritte:

1.  **Assignment Step:** Zuordnung jedes Datenpunktes zum nächstgelegenen Zentroiden (basierend auf euklidischer Distanz).
2.  **Update Step:** Neuberechnung der Zentroide als Mittelwert aller dem Cluster zugeordneten Punkte.

## Implementierungsdetails

### 1\. Datensynthese

Zur Verifikation des Algorithmus werden synthetische, isotrope Gauß-Blobs generiert (`n_samples=5000`, `centers=7`). Dies erlaubt einen direkten Vergleich zwischen dem algorithmischen Ergebnis und der Ground Truth.

### 2\. Robuste Initialisierung (Custom Logic)

Die Konvergenz von K-Means ist stark abhängig von der Startposition der Zentroide. Eine naive zufällige Initialisierung führt oft zu suboptimalen lokalen Minima.

  * **Lösung:** Implementierung der Funktion `gen_centers_with_min_distance`.
  * **Logik:** Startzentroiden werden so gewählt, dass sie eine stochastische Mindestdistanz (basierend auf der Standardabweichung der Datenverteilung) zueinander einhalten. Dies erhöht die Wahrscheinlichkeit, globale Optima zu finden, signifikant.

### 3\. Hyperparameter-Optimierung (Model Selection)

Da die Anzahl der Cluster $k$ in realen Daten oft unbekannt ist, implementiert dieses Projekt eine automatisierte Bestimmung des optimalen $k$.

  * Berechnung der mittleren Varianz für $k \in [1, 10]$.
  * Analyse der Änderungsrate (Gradient) der Varianzkurve zur Identifikation des "Elbow-Points".

## Ergebnisse

### Cluster-Konvergenz

Die folgende Abbildung zeigt das finale Partitionierungsergebnis für das als optimal identifizierte $k=7$. Die Visualisierung demonstriert die Fähigkeit des Algorithmus, die zugrundeliegende Struktur der Verteilung korrekt zu rekonstruieren.

> <img width="761" height="477" alt="image" src="https://github.com/user-attachments/assets/9ed4ee46-cf5b-46ec-aae9-d67ace0057e1" style="margin-left: 0; display: block;"/>


### Varianzanalyse (Elbow Method)

Zur Bestimmung der optimalen Clusteranzahl wird die Varianzreduktion gegen die Modellkomplexität geplottet. Der Algorithmus bricht ab, sobald der Informationsgewinn durch zusätzliche Cluster einen Schwellenwert unterschreitet.

> <img width="720" height="480" alt="image" src="https://github.com/user-attachments/assets/67f0166b-6e88-4880-9d64-17ee1ed1f0b1" style="margin-left: 0; display: block;"/>


## Technologien

  * **Python 3.x**: Core Logic.
  * **NumPy**: Vektorisierte Berechnung der euklidischen Distanzen (Performance-Optimierung).
  * **Matplotlib**: Visualisierung der Ergebnisse.
  * **Scikit-Learn**: Ausschließlich zur Generierung der synthetischen Testdaten (`make_blobs`).

## Ausführung

Das Projekt ist als Jupyter Notebook strukturiert, um den explorativen Charakter der Datenanalyse zu unterstützen.

```bash
# Clone repository
git clone [repo-link]

# Install dependencies
pip install numpy matplotlib scikit-learn tqdm colored

# Run notebook
jupyter notebook kmeans.ipynb
```
