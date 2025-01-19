# Picture Colorizing


| **Zasób**                 | **Adres**                                                                                                                  |
| ------------------------- | -------------------------------------------------------------------------------------------------------------------------- |
| **Repozytorium projektu** | [github.com/jacekstrzalkowski/picturecolorizing](https://github.com/jacekstrzalkowski/picturecolorizing)                   |
| **Kaggle notebook**       | [kaggle.com/code/jacekstrzakowski/colorizing/](https://www.kaggle.com/code/jacekstrzakowski/colorizing/)                   |

Sztuczne kolorowanie obrazów jest rozumiane jako klasyfikacja obrazu w skali szarości $\mathbf{X} \in \mathbb{R}^{H\times W\times 1}$ do obrazu kolorowego $\mathbf{\tilde{X}}$: tablicy 2D $(H,W)$ trójek $(\mathbf{\tilde{X}_{R}}, \mathbf{\tilde{X}_{G}}, \mathbf{\tilde{X}_{B})}$, $\mathbf{\tilde{X}} \in \mathbb{R}^{H\times W\times 3}$.

Postępując podobnie jak w [@zhangColorfulImageColorization2016], przechodzimy z reprezentacji `RGB` obrazu do reprezentacji kolorów przestrzeni barw `LAB`. W przestrzeni LAB składowa jasności $L$ jest oddzielona od składowych: $a$ - barwy czerwono - zielone, $b$ - niebiesko żółte,
$$\mathbf{\tilde{X}}_{Lab} = \{L, \tilde{a}, \tilde{b}\}, \quad \mathbf{\tilde{X}}_{Lab} \in \mathbb{R}^{H \times W \times 3}.$$
Sieć neuronowa $M$ w działaniu na obraz $\mathbf{X}$ zwraca $\mathbf{\tilde{X}}_{Lab}$, który następnie jest poddawany odwrotnej transformacji z przestrzeni `Lab` do `RGB`. Zauważmy, że pozwala to ująć problem kolorowania obrazów jako klasyfikacji jedynie dwóch kanałów $(a,b)$, podczas gdy jasność pozostaje dostępna jako wejście.
