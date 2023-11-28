# WinAssist Plugin Developer Documentation

## Einführung

Willkommen zur WinAssist Plugin-Entwicklerdokumentation! Diese Dokumentation bietet Informationen und Richtlinien für Entwickler, die Plugins für die WinAssist-Anwendung erstellen möchten.

## Inhaltsverzeichnis

1. [Grundlagen](#grundlagen)<br>
   1.1. [Was sind WinAssist-Plugins?](#was-sind-winassist-plugins)<br>
   1.2. [Voraussetzungen](#voraussetzungen)<br>

2. [Plugin-Entwicklung](#plugin-entwicklung)<br>
   2.1. [ICommandPlugin-Schnittstelle](#icommandplugin-schnittstelle)<br>
   2.2. [Namenskonventionen](#namenskonventionen)<br>
   2.3. [Lebenszyklus von Plugins](#lebenszyklus-von-plugins)<br>
   2.4. [Behandlung von Ausnahmen](#behandlung-von-ausnahmen)<br>
   
3. [Plugin-Integration](#plugin-integration)<br>
   3.1. [Ordner für Plugins](#ordner-für-plugins)<br>
   3.2. [Laden von Plugins](#laden-von-plugins)<br>
   3.3. [Berechtigungen und Sicherheit](#berechtigungen-und-sicherheit)<br>

4. [Beispiel-Plugin](#beispiel-plugin)<br>
   
## Grundlagen

### Was sind WinAssist-Plugins?

WinAssist-Plugins sind Erweiterungsmodule, die die Funktionalität der WinAssist-Anwendung erweitern können. Diese Plugins können benutzerdefinierte Befehle und Aktionen hinzufügen.

### Voraussetzungen

- Grundlegende Kenntnisse in der C#-Programmierung
- Visual Studio oder ein anderer C#-Entwicklungsumgebung

## Plugin-Entwicklung

### ICommandPlugin-Schnittstelle

Plugins müssen die `ICommandPlugin`-Schnittstelle implementieren. Diese Schnittstelle stellt sicher, dass grundlegende Methoden für die Plugin-Integration vorhanden sind. Die [WinAssist-PluginLibrary.dll](https://github.com/lulv3z/WinAssist-PluginLibrary/releases/tag/V.1) steht für Entwickler zur Verfügung, um den Plugin-Entwicklungsprozess zu vereinfachen.

```csharp
public interface ICommandPlugin
{
    string PluginName { get; }
    string ExecuteCommand(string command);
}

```

Die PluginName-Eigenschaft gibt den Namen des Plugins zurück, und die ExecuteCommand-Methode verarbeitet die Befehle.

## Namenskonventionen

Es wird empfohlen, Plugins mit eindeutigen Namen zu versehen und die Namenskonvention "PluginNamePlugin" zu verwenden.

Beispiel: CalculatorPlugin, HelloWorldPlugin

## Lebenszyklus von Plugins

Plugins durchlaufen einen Lebenszyklus, der das Laden, Ausführen und Entladen umfasst. Beachte, dass Plugins bei Bedarf geladen und entladen werden können. Achte darauf, Ressourcen ordnungsgemäß freizugeben.

## Behandlung von Ausnahmen

Behandle Ausnahmen in deinem Plugin-Code. Fehler in einem Plugin dürfen nicht die Stabilität der WinAssist-Anwendung beeinträchtigen. Logge Fehlermeldungen und informiere den Benutzer, wenn nötig.

# Plugin-Integration
## Ordner für Plugins

Erstelle einen Ordner in deinem WinAssist-Projekt, um Plugin-DLLs zu speichern. Standardmäßig kann dies "PluginFolder" sein.

## Laden von Plugins

WinAssist kann Plugins aus einem bestimmten Ordner laden. Stelle sicher, dass die DLL-Dateien im Plugin-Ordner vorhanden sind.

Beispielcode in WinAssist:
```csharp
List<ICommandPlugin> loadedPlugins = PluginManager.LoadPlugins("PluginFolder");
foreach (ICommandPlugin plugin in loadedPlugins)
{
    Console.WriteLine($"Geladenes Plugin: {plugin.PluginName}");
}
```

## Berechtigungen und Sicherheit

Achte darauf, dass Plugins nur auf die benötigten Ressourcen zugreifen können. Implementiere gegebenenfalls Sicherheitsmechanismen, um unerwünschte Aktionen zu verhindern.

# Beispiel-Plugin

Hier ist ein einfaches Beispiel für ein Plugin, das die ICommandPlugin-Schnittstelle implementiert:
```csharp
public class HelloWorldPlugin : ICommandPlugin
{
    public string PluginName => "HelloWorldPlugin";

    public string ExecuteCommand(string command)
    {
        if (command.Equals("hello", StringComparison.OrdinalIgnoreCase))
        {
            return "Hallo von HelloWorldPlugin!";
        }

        return string.Empty; // Der Befehl wurde nicht vom Plugin verstanden
    }
}
```
Dieses Beispiel-Plugin reagiert auf den Befehl "hello". Es gibt "Hallo von HelloWorldPlugin!" zurück, wenn dieser Befehl erkannt wird.
