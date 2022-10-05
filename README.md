# Estudio-Biometría-de-Pasos---IE0435
Se incluyen los archivos .arff utilizados para el análisis del proyecto biometría de pasos, además, a continuación, una explicación de la metodología empleada.

Se partió de 4 archivos de audio, uno de aproximadamente 5 segundos para los datos de test y uno de aproximadamente 20 minutos para los datos de train, esto para dos personas distintas caminando en un círculo con radio de unos 150cm con el micrófono como centro. 

Inicialmente, se procedió utilizando la herramienta Sox y el siguiente comando a generar archivos de 5s.
~~~
     sox file_in . mp3 file_out . mp3 trim 0 5 : newfile : restart

~~~

Luego con estos archivos en una carpeta se procedió a extrer los vectores de características para cada caso, haciendo uso de la herramienta pyAudioAnalysis.


~~~
python3 ./pyAudioAnalysis/audioAnalysis.py featureExtractionDir -i ./clase1 -mw 1.0 -ms 1.0 -sw 0.050 -ss 0.050
~~~

Nota: Lo anterior suponiendo que se ejecuta desde la carpeta de pyAudioAnalysis-master



Para convertir las características extraídas en un archivo en formato csv, con la clase al final de cada vector, se utilizó el siguiente comando en terminal, para las distintas clases:
~~~
for i in *wav_st.csv; do awk -F, '{for(i=1; i<=NF; i++) {a[i]+=$i; if($i!="") b[i]++}}; END {for(i=1; i<=NF; i++) printf "%s,%s", a[i]/b[i], (i==NF?ORS:OFS)}' "$i" | awk '{$(NF+1)="clase1"}1' ; done > datos_clase1
~~~


Finalmente se construyó el archivo .arff con la concatenación de los datos anteriores y el encabezado correspondiente para este tipo de archivos.
