# Práctica 7: Buses de comunicación III (I2S)

## Ejercicio práctico 1 reproduccion desde memória interna

Script:

```cpp
#include "AudioGeneratorAAC.h"
#include "AudioOutputI2S.h"
#include "AudioFileSourcePROGMEM.h"
#include "sampleaac.h"

AudioFileSourcePROGMEM *in;
AudioGeneratorAAC *aac;
AudioOutputI2S *out;

void setup()
{
    Serial.begin(115200);

    in = new AudioFileSourcePROGMEM(sampleaac, sizeof(sampleaac));
    aac = new AudioGeneratorAAC();
    out = new AudioOutputI2S();
    out -> SetGain(0.125);
    out -> SetPinout(26,25,22);
    aac->begin(in, out);
}

void loop()
{
    if (aac->isRunning())
    {
        aac->loop();
    }
    else
    {
        aac -> stop();
        Serial.printf("Sound Generator\n");
        delay(1000);
    }
}
```

Para el código usamos la [biblioteca de audio ESP8266 de Earle F. Philhower](https://github.com/earlephilhower/ESP8266Audio).

Explicación: Reproduce un archivo de audio AAC a través de un reproductor de AAC que proporcionan las librerias en la ESP32. Primero en el setup() se declaran ciertos parametros para cuendo se realize la reproduccion, como el nivel de ganancia, el archivo AAC, los pines. Posteriormente se inicializa la reproduccion del archivo von el begin al final del setup(). La funcion del loop() es controlar que el archivo esta reproduciondeose o si ya ha terminado esta reproducción.

## Ejercicio práctico 2 reproducir un archivo WAVE en ESP32 desde una tarjeta SD externa

```cpp
#include "Audio.h"
#include "SD.h"
#include "FS.h"

// Digital I/O used
#define SD_CS          5
#define SPI_MOSI      23
#define SPI_MISO      19
#define SPI_SCK       18
#define I2S_DOUT      25
#define I2S_BCLK      27
#define I2S_LRC       26

Audio audio;

void setup()
{
    pinMode(SD_CS, OUTPUT);
    digitalWrite(SD_CS, HIGH);
    SPI.begin(SPI_SCK, SPI_MISO, SPI_MOSI);
    Serial.begin(115200);
    SD.begin(SD_CS);
    audio.setPinout(I2S_BCLK, I2S_LRC, I2S_DOUT);
    audio.setVolume(10); // 0...21
    audio.connecttoFS(SD, "Ensoniq-ZR-76-01-Dope-77.wav");
}

void loop()
{
    audio.loop();
}

// optional
void audio_info(const char *info)
{
    Serial.print("info        "); Serial.println(info);
}
void audio_id3data(const char *info)  //id3 metadata
{
    Serial.print("id3data     ");Serial.println(info);
}
void audio_eof_mp3(const char *info)  //end of file
{
    Serial.print("eof_mp3     ");Serial.println(info);
}
void audio_showstation(const char *info)
{
    Serial.print("station     ");Serial.println(info);
}
void audio_showstreaminfo(const char *info)
{
    Serial.print("streaminfo  ");Serial.println(info);
}
void audio_showstreamtitle(const char *info)
{
    Serial.print("streamtitle ");Serial.println(info);
}
void audio_bitrate(const char *info)
{
    Serial.print("bitrate     ");Serial.println(info);
}
void audio_commercial(const char *info)  //duration in sec
{
    Serial.print("commercial  ");Serial.println(info);
}
void audio_icyurl(const char *info)  //homepage
{
    Serial.print("icyurl      ");Serial.println(info);
}
void audio_lasthost(const char *info)  //stream URL played
{
    Serial.print("lasthost    ");Serial.println(info);
}
void audio_eof_speech(const char *info)
{
    Serial.print("eof_speech  ");Serial.println(info);
}
```

Para este script utilizaremos otra [libreria externa](https://github.com/schreibfaul1/ESP32-audioI2S).

Explicación: Se asemeja un poco al planteamiento del codigo anterior en la parte del setup, defines el lector SD, luego con el reproductor definiendo los pines de buses I2S, el volumen (que puede variar de 0 a 21), entonces al final define el archivo de audio .rar a reproducir en la SD. el loop() hace lo mismo que en el aterior que es controlar si el audio esta en reproduccion o ha terminado. Ademas se han añadido subprogramas adicionales en los que poder proyectar en el terminal serie datos de la reproduccion.

