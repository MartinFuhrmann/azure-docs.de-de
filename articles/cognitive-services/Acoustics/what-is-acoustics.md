---
title: Übersicht über Projekt Akustik
titlesuffix: Azure Cognitive Services
description: Projekt Akustik ist eine Akustik-Engine für interaktive 3D-Erfahrungen, in der auf Wellen basierende Physiksimulationen mit Baking mit interaktiven Entwurfssteuerelementen integriert sind.
services: cognitive-services
author: NoelCross
manager: nitinme
ms.service: cognitive-services
ms.subservice: acoustics
ms.topic: overview
ms.date: 03/20/2019
ms.author: noelc
ROBOTS: NOINDEX
ms.openlocfilehash: 65678f08399f378b8580eed79e49197dd4d84c64
ms.sourcegitcommit: 7f6d986a60eff2c170172bd8bcb834302bb41f71
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/27/2019
ms.locfileid: "71351149"
---
# <a name="what-is-project-acoustics"></a>Was ist „Projekt: Akustik“?
Projekt Akustik ist eine Wellenakustik-Engine für interaktive 3D-Erfahrungen. Sie modelliert Welleneffekte wie Verdeckungs-, Hindernis-, Portal- und Halleffekte in komplexen Szenen ohne manuelle Zonenmarkierung oder CPU-intensives Raytracing. Außerdem schließt sie eine Integration von Spiele-Engines und Audiomiddleware ein. Die Philosophie hinter Projekt Akustik ist vergleichbar mit einer statischen Beleuchtung: Das Baking einer detaillierten Physik wird offline ausgeführt, um eine physische Baseline bereitzustellen, und die künstlerischen Ziele für die Akustik Ihrer virtuellen Welt werden mithilfe einer einfachen Runtime mit ausdrucksstarken Entwurfssteuerelementen erreicht.

![Screenshot von Gears of War 4 mit Akustikvoxeln](media/gears-with-voxels.jpg)

## <a name="using-wave-physics-for-interactive-acoustics"></a>Verwenden von Wellenphysik für interaktive Akustik
Auf Strahlen basierende Akustikmethoden können eine Überprüfung auf Verdeckung mit einer einzelnen Quelle-zu-Zuhörer-Strahlenausdehnung überprüfen oder den Hall durch Abschätzen des Volumens der lokalen Szene mit wenigen Strahlen erzeugen. Diese Techniken sind jedoch nicht immer zuverlässig, da ein Kieselstein für eine ebenso starke Verdeckung sorgt wie ein großer Felsblock. Mit Strahlen lässt sich die Krümmung von Tönen um Objekte herum – ein als Beugung bezeichnetes Phänomen – nicht abbilden. Die Simulation in Projekt Akustik erfasst diese Effekte mithilfe einer auf Wellen basierenden Simulation. Die Akustik ist besser vorhersagbar, präziser und nahtlos.

Die zentrale Innovation von Projekt Akustik besteht darin, eine auf echten Schallwellen basierende akustische Simulation mit herkömmlichen Konzepten des Sounddesigns zu verbinden. Die Simulationsergebnisse werden in herkömmliche Audio-DSP-Parameter für Verdeckung, Portale und Hall übersetzt. Der Designer nutzt Steuerelemente für diesen Übersetzungsvorgang. Weitere Informationen zu den Kerntechnologien hinter Projekt Akustik finden Sie auf der [Forschungsprojektseite](https://www.microsoft.com/en-us/research/project/project-triton/).

![Animation mit einem horizontalen 2D-Segment mit Wellenausbreitung in einer Szene](media/wave-simulation.gif)

## <a name="video-presentation-from-gdc-2019-30-min"></a>Video Präsentation von der GDC 2019 (ca. 30 Minuten)
[![Projekt Akustik: Video](https://img.youtube.com/vi/uY4G-GUAQIE/0.jpg)](https://www.youtube.com/watch?v=uY4G-GUAQIE "Zum Abspielen des Videos klicken")

## <a name="setup"></a>Einrichtung
Die [Unity-Integration von Projekt Akustik](unity-integration.md) unterstützt Drag & Drop und enthält ein Unity-Audio-Engine-Plug-In. Erweitern Sie die Audioquellen-Steuerelemente von Unity, indem Sie eine Komponente für Projekt Akustik-C#-Steuerelemente an jedes Audioobjekt anfügen.

Die [Unreal-Integration von Projekt Akustik](unreal-integration.md) enthält Editor- und Spiel-Plug-Ins für Unreal sowie ein Wwise-Mixer-Plug-In. Eine benutzerdefinierte Audiokomponente erweitert die vertrauten Wwise-Funktionen in Unreal um Liveakustik-Entwurfssteuerelemente. Die Entwurfssteuerelemente sind außerdem in Wwise im Mixer-Plug-In verfügbar.

## <a name="workflow"></a>Workflow
* **Vorabbaking:** Beginnen Sie mit der Einrichtung des Baking, indem Sie auswählen, welche Geometrie auf Akustik reagieren soll, z. B. durch Ignorieren von Lichtschächten. Bearbeiten Sie dann automatische Materialzuweisungen, und wählen Sie Navigationsbereiche aus, um das Zuhörersampling zu steuern. Es findet keine manuelle Markierung für Hall-/Portal-/Raumzonen statt.
* **Baking:** In einem lokal ausgeführten Analyseschritt werden die Umwandlung in Voxel und andere geometrische Analysen entsprechend der Auswahl oben in der Szene umgesetzt. Die Ergebnisse werden im Editor visualisiert, um die Szeneneinrichtung zu überprüfen. Bei der Bakingübermittlung werden die Voxeldaten an Azure gesendet, und Sie erhalten eine Akustikspielressource.
* **Runtime:** Laden Sie die Ressource in Ihren Level, dann können Sie sich die Akustik im Level anhören. Entwerfen Sie die Akustik live im Editor mit abgestuften Steuerelementen für jede Quelle. Die Steuerelemente können auch über Levelskripts gesteuert werden.

## <a name="runtime-platforms"></a>Runtime-Plattformen
Die Projekt Akustik-Runtime-Plug-Ins können derzeit auf den folgenden Plattformen bereitgestellt werden:
* Windows
* macOS
* Android
* Xbox One

## <a name="editor-platforms"></a>Editor-Plattformen
Das Projekt Akustik-Editor-Plug-In ist für die folgenden Plattformen verfügbar:
* Windows
* macOS (nur Unity)

## <a name="download"></a>Download
* [Projekt Akustik (Unity-Plug-Ins und -Beispiele)](https://www.microsoft.com/en-us/download/details.aspx?id=57346)
* [Projekt Akustik (Unreal- und Wwise-Plug-Ins und -Beispiele)](https://www.microsoft.com/download/details.aspx?id=58090)
  * Um Binärdateien und sonstigen Support für die Xbox zu erhalten, kontaktieren Sie uns über das [Forum](https://github.com/microsoft/ProjectAcoustics/issues).

## <a name="contact-us"></a>Kontakt
* [Erläuterung zum Projekt Akustik und Meldung von Problemen](https://github.com/microsoft/ProjectAcoustics/issues)

## <a name="next-steps"></a>Nächste Schritte
* Probieren Sie einen [Projekt Akustik-Schnellstart für Unity](unity-quickstart.md) oder [Unreal](unreal-quickstart.md) aus.
* Erkunden Sie die [Sounddesignphilosophie von Projekt Akustik](design-process.md).

