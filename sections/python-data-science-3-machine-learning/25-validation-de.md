# Validierung

## Train-Test Split

Um zu validieren, ob ein Verfahren ein passendes Ergebnis liefert:

Die Daten werden in Trainingsdaten und Testdaten unterteilt. Die Testdaten dienen nur zur Validierung.

## Train-Test Split

Wie gut kategorisiert ein bestimmter Algorithmus die Iris-Daten?

```py
from sklearn.model_selection import train_test_split
from sklearn import metrics

X_train, X_test, Y_train, Y_test = train_test_split(X, Y)

# ...

Y_prediction = model.predict(X_test)
print(metrics.accuracy_score(Y_test, Y_prediction))
```

optionaler Parameter: `test_size` (Standardwert `0.25`)

## Kreuzvalidierung

Bei der Kreuzvalidierung (cross-validation) werden die Daten wiederholt in unterschiedliche Trainings- und Testdaten unterteilt, sodass jeder Eintrag einmal in den Testdaten vorkommt.

```py
from sklearn.model_selection import cross_validate

test_results = cross_validate(
    model, X, y, cv=5, scoring="accuracy"
)
print(test_results["test_score"])
```

## Validierungsmetriken

Klassifizierung:

- accuracy metrics
  - accuracy
  - confusion matrix
- Metriken, die auf true/false positives/negatives basieren
  - precision
  - recall
  - f-score
  - ROC und AUC
- probabilistische Metriken
  - Kreuzentropie

Regression:

- Mittlere quadratische Abweichung
- Bestimmtheitsmaß (R²)

## Klassifizierungsmetriken

Beispiel:

Ein Korb von Früchten enthält 10 Äpfel, 10 Orangen und 10 Pfirsiche

Ein Klassifizierungsalgorithmus liefert diese Ergebnisse:

- Klassifizierung von Äpfeln: 8 als Äpfel, 0 als Orangen, 2 als Pfirsiche
- Klassifizierung von Orangen: 10 als Orangen
- Klassifizierung von Pfirsichen: 1 als Apfel, 0 als Orangen, 9 als Pfirsiche

## Accuracy metrics

**accuracy**: relativer Anteil korrekter Klassifizierungen (im Beispiel: 27/30=0.9)

**confusion matrix**: Tabelle mit Klassifizierungen für jede Kategorie

|         | apples | oranges | peaches |
| ------- | ------ | ------- | ------- |
| apples  | 8      | 0       | 2       |
| oranges | 0      | 10      | 0       |
| peaches | 1      | 0       | 9       |

## Metriken, die auf true/false positives/negatives basieren

Binäre Klassifizierung: ist eine Frucht ein Apfel oder ist sie kein Apfel?

- true positive: ein Apfel wird als Apfel klassifiziert
- true negative: ein Pfirsich wird als _kein_ Apfel klassifiziert
- false positive: ein Pfirsich wird als Apfel klassifiziert (Fehler erster Art)
- false negative: ein Apfel wird als _kein_ Apfel klassifiziert (Fehler zweiter Art)

## Metriken, die auf true/false positives/negatives basieren

**precision** (Genauigkeit) = 8/9=0.889 (8 von 9 Früchten, die als Äpfel klassifiziert wurden, sind tatsächlich Äpfel)

**recall** (Trefferquote) = 8/10=0.8 (8 von 10 Äpfeln wurden als Äpfel erkannt)

precision = true positives / predicted positives

recall = true positives / condition positives

siehe auch: [Precision and recall auf der englischsprachigen Wikipedia](https://en.wikipedia.org/wiki/Precision_and_recall)

## Metriken, die auf true/false positives/negatives basieren

_precision_ und _recall_ haben unterschiedliche Relevanz in verschiedenen Szenarien

Beispiel: Beim Klassifizieren von E-mails als Spam ist _precision_ besonders wichtig (vermeiden, eine E-mail fälschlicherweise als Spam zu klassifizieren)

## Metriken, die auf true/false positives/negatives basieren

**f-score** = harmonisches Mittel zwischen _precision_ und _recall_

## Metriken, die auf true/false positives/negatives basieren

**ROC** (Receiver Operating Characteristic)

= Metrik, die _true positives_ und _false positives_ wiederspiegelt

Ein Klassifizierungsalgorithmus könnte bezüglich seiner _true positives_-Quote und _false positives_-Quote fein eingestellt werden:

- Option 1: 60% true positives-Quote, 0% false positives-Quote
- Option 2: 70% true positives-Quote, 5% false positives-Quote
- Option 3: 80% true positives-Quote, 25% false positives-Quote
- Option 4: 90% true positives-Quote, 55% false positives-Quote
- Option 5: 95% true positives-Quote, 90% false positives-Quote

## Metriken, die auf true/false positives/negatives basieren

Die ROC kann als Kurve dargestellt werden; je größer die Fläche unter der Kurve (area under the curve, AUC), desto besser die Klassifikation

## Metriken, die auf true/false positives/negatives basieren

Bestimmen der ROC mit scikit-learn:

```py
false_positive_rates, true_positive_rates, thresholds = metrics.roc_curve(
    y_test,
    classifier.predict_proba(X_test)[: 1]
)
```

Zeichnen der ROC:

```py
plt.plot(false_positive_rate, true_positive_rate)
```

Bestimmen der AUC:

```py
auc = metrics.auc(false_positive_rates, true_positive_rates)
```

## Probabilistische Metriken

**Kreuzentropie** (log loss): Misst, wie gut ein Modell einer Wahrscheinlichkeitsverteilung die tatsächliche Wahrscheinlichkeitsverteilung annähert

relevant bei _neuronalen Netzwerken_ und _logistischer Regression_

## Regressionsmetriken

**mittlere quadratische Abweichung**

**Bestimmtheitsmaß (R²)**:

vergleicht die mittlere quadratische Abweichung der Regression mit der Varianz der Eingangsdaten

- R²=1 - perfekte Interpolation
- R²=0 - Interpolation ist nicht besser als das Verwenden des Durchschnitts
- R²<0 - schlechter als das Verwenden des Durchschnitts

## Validierungsmetriken in scikit-learn

Klassifizierung:

- _accuracy_score_
- _confusion_matrix_
- _precision_recall_fscore_support_
- _log_loss_
- _roc_curve_
- _roc_auc_

Regression:

- _mean_squared_error_
- _r2_score_

Siehe auch <https://scikit-learn.org/stable/modules/classes.html#module-sklearn.metrics>

## Validierunsmetriken in Keras

- _accuracy_
- _categorical_crossentropy_
- _sparse_categorical_crossentropy_
- _precision_
- _recall_
- _auc_
- _mean_squared_error_

Siehe auch <https://keras.io/api/metrics/>

## Validierung

Aufgabe: Validierung der Iris-Klassifizierung
