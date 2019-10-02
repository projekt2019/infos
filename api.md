# Verbindung zwischen Clients und Server

## Instanz erstellen

Für jede Verbindung wird eine IP und ein PORT benötigt. Diese werden jeweils im Konstruktor festgesetzt oder(wenn nicht angegeben) haben einen vordefinierten Wert.
```vb
' Client
Dim Connector as ClientConnector = new ClientConnector("127.0.0.1",8080)
' ODER
new ClientConnector() ' -> Standartwerte
```
Bei dem Server ist in der Regel nur der Port relevant, da die IP die eigene ist(0.0.0.0)

## Events

Es gibt verschiedene Events, die auftreten können, nachdem eine Verbindung hergestellt wurde. Die Eventhandler sind EventNotifier(=Liste von Prozeduren), die mittels `AddressOf` übergeben werden:

```vb
Connector.OnRecieve.addHandler(AddressOf recieveHandler)

Private Sub recieveHandler(msg as String)
  ... ' mach was mit dem String
End Sub
```

* OnConnect - Wird für jede neue Verbindung(mit Client für Server) aufgerufen
* OnRecieve - Wird bei empfangender Nachricht aufgerufen(mit Übergabe der Nachricht als String und evtl. des Clients)
* OnClose - Wird ausgeführt, wenn der Client oder der Server die Verbindung schließt

## Verbindung aufbauen

Für die jeweiligen Verbindungen wird eine endlose Schleife(While True) benötigt. Der Server läuft daher unendlich weiter. 
Für die Clients wird das außerhalb des Hauptprogramms ausgeführt, sodass dieses nicht stockt.

Die `.connect` Methode sollte daher immer am Ende, nachdem alle Events und Werte gesetzt wurden, aufgerufen werden.
Erst dann können Nachrichten übertragen werden.

```vb
Connector.connect()
```

# EventNotifier

Ein EventNotifier hält eine Liste von Methoden, die alle mit einer übergeordneten Prozedur(Notify) benachrichtigt und ausgeführt werden.
Es ermöglicht mehrere Empfängermethoden mit einem Ereignes(z.B OnRecieve) aufzurufen.
