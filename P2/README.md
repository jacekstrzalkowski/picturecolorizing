## Zdefiniowanie problemu

Problem może być opisany jako klasyfikacja wartości $a$, $b$ dla danego piksela, gdzie możliwymi klasami będą wartości kolorów.

![](kolorowanie.png)

---

Dla obrazu zdefiniowanego poprzez wartości oświetlenia $\mathbf{X_{L}}\in \mathbb{R}^{H\times W\times 1}$ w jaki sposób przypisać (ang. *halucinate*) obraz zdefiniowany poprzez `RGB` $\mathbf{\tilde{X}}\in \mathbb{R}^{H\times W\times 3}$ 
$$
f: \mathbf{X_{L}}\to(\mathbf{\tilde{X}_{a}}, \mathbf{\tilde{X}_{b}})
.$$
Obraz kolorowy $\mathbf{\tilde{X}}$: trójka $(\mathbf{X_{L}}, \mathbf{\tilde{X}_{a}}, \mathbf{\tilde{X}_{b}})$ , a kanały $\mathbf{\tilde{X}_{a}}$, $\mathbf{\tilde{X}_{b}}$ `chrominance channel` [@baldassarreDeepKoalarizationImage2017].

## Analiza literatury

Problem kolorowania szarych obrazów jest problemem, dla którego opracowano wiele klasycznych tj. nieMLowych rozwiązań[@zegerGrayscaleImageColorization2021],[@vitoriaChromaGANAdversarialPicture2020].

Przedstawione rozwiązanie wykorzystujące sieć CNN [@zhangColorfulImageColorization2016] było testowane z wykorzystaniem

- zbieżność matematyczna klasyfikacji koloru kolorowego oryginału i obrazu uzyskanego z kolorowania. Wynik z kompensacją: $67.3\%$, bez kompensacji $89.5\%$.[^rebel]
- testów *prawdziwości* zdjęć (ang. *perceptual realism*). Użytkownicy byli proszeni o ocenienie, czy zdjęcie było kolorowane sztucznie. Uzyskano wynik $\mathbf{32.3\pm2.2}\%$.

--------------

- Wykorzystanie sztucznie kolorowanych obrazów do typowej klasyfikacji klas obiektów `ImageNet`[^klas]. Uzyskano $56.0\%$.

[^rebel]: Kompensacja kolorów jest metodą stosowaną celem ograniczenia zjawiska preferowania przez sieć predykcji popularnych kolorów, skutkującego niewystępowaniem rzadkich kolorów, co powoduje, że takie obrazy wypadają gorzej w testach wykorzystujących ludzi.
[^klas]: Klasyfikacja czy zdjęcie przedstawia obiekt.

-------------

Problem może być również wykorzystany przy użyciu sieci **GAN** i *regularyzacji percepcyjnej* - sieć pracuje na poziomie wyższych cech niż pojedyncze piksele [@vitoriaChromaGANAdversarialPicture2020]. Do tego celu autorzy wykorzystują przetrenowany model `VGG-16`.

Poza testami typu *perceptual realism*, przeprowadzono testy numeryczne: PSRN,
$$
\text{PSNR} = 10 \cdot \log_{10} \left( \frac{MAX_I^2}{\text{MSE}} \right)
,$$
gdzie $MAX$ - maksymalna wartość piksela, $MSE$ - błąd średnio kwadratowy liczony od wartości pikseli na porównywanych obrazach.

Dla modelu ChromaGAN uzyskano wynik lepszy $25.57$ dB niż dla sieci z [@zhangColorfulImageColorization2016], gdzie było to $22.04$ dB.

## Dataset

Sieć będzie trenowana na obrazach w skali szarości, które zostały z konwersji obrazów kolorowych. Uzyskanie datasetu może sprowadzić się więc do zebrania odpowiedniej ilości kolorych obrazów (czyli użycia datasetu np. do problemu klasyfikacji) i wykorzystania np. programu ImageMagick
```bash
$ convert color.png -colorspace Gray gray.png
```
Użyty zostanie zbiór danych `ImageNet`[^ImageNet].  Zbiór ten zawiera ponad milion obrazków, które mogą posłużyć do treningu,.

[^ImageNet]: [https://www.kaggle.com/c/imagenet-object-localization-challenge/overview](https://www.kaggle.com/c/imagenet-object-localization-challenge/overview)

## Propozycje rozwiązania

- Zostanie skonstruowana i wytrenowana sieć CNN zainspirowana [@zhangColorfulImageColorization2016]. Sprawdzone zostaną wymienione funkcje aktywacji i kosztu wymieniane w literaturze odwołującej się do [@zhangColorfulImageColorization2016].
- Uwzględnione będą użyte w [@zhangColorfulImageColorization2016] poprawki na nierównowagę klas (Patrz *kompensacja kolorów*).

![Schemat sieci CNN użytej do kolorowania](nn.png)

--------------

- Jeśli to możliwe, przetestowany będzie algorytm GAN opisany w [@vitoriaChromaGANAdversarialPicture2020] wykorzystujący przetrenowane sieci do detekcji cech.
- Zostanie przygotowany interfejs w przeglądarce, który umożliwi przeprowadzanie testów *perceptual realism*.

## Bibliografia
