# Lern-Bericht
von Flynn Guerra

## Einleitung

In diesem gesamten Modul ging es darum, wie man eine Webseite sicher gestalten kann, um die Sicherheitslücken so gering wie möglich zu behalten.
In diesem Lernbericht geht es darum, wie man eine Webseite vor einer SQL-Injection schützt.

## Was habe ich gelernt?

Ich habe in diesem Projekt gelernt, wie man eine SQL-Interpreter Injection anhand von preparedStatements abfängt und so eine Webseite davor sichert.

## Beschreibung
Ursprünglicher Code:
```Java
      public int insert(News news) {
        final String sql = "INSERT INTO news (posted, header, detail, author, is_admin_news) VALUES ('" + new java.sql.Timestamp(news.getPosted().getTime()) + "','" + news.getHeader() + "','" + news.getDetail() + "','" + news.getAuthor() + "'," + (news.getIsAdminNews() ? "1" : "0") + ")";
        int id = 0;

        try (Statement stmt = DbAccess.getConnection().createStatement()) {
            stmt.execute(sql);
            id = stmt.getGeneratedKeys().getInt(1);
        } catch (SQLException e) {
            Logger.getLogger(NewsDAO.class.getName()).log(Level.SEVERE, null, e);
        }

        return id;

    }
```
Abgeänderter und sicherer Code mit preparedStatements:
```Java
      public int insert(News news) {
        final String sql = "INSERT INTO news (posted, header, detail, author, is_admin_news) VALUES (?, ?, ?, ?, ?)";
        int id = 0;

        try (Connection conn = this.connect();
                PreparedStatement pstmt = conn.prepareStatement(sql)) {
            pstmt.setTimestamp(1, new java.sql.Timestamp(news.getPosted().getTime()));
            pstmt.setString(2, news.getHeader());
            pstmt.setString(3, news.getDetail());
            pstmt.setString(4, news.getAuthor());
            pstmt.setString(5, (news.getIsAdminNews() ? "1" : "0"));
            pstmt.executeUpdate();
        } catch (SQLException e) {
            Logger.getLogger(NewsDAO.class.getName()).log(Level.SEVERE, null, e);
        }

        return id;

    }
```

Die Sicherheitslücke bei dem obigen Code-Abschnitt ist, dass man bei diesem durch eine simple Manipulation des Eingabewertes sich als Admin anzeigen lassen kann. Dies ist hier natürlich harmlos und verändert nur die Farbe, in welcher die News angezeigt werden, aber wenn dies bei einer richtigen Webseite der Fall sein würde, könnte man damit sehr viel Falsches anstellen.
Um in diesem Fall den Benutzer als Administrator darstellen zu lassen, muss man das Eingabefeld bei der News Page anpassen und folgendes hineinschreiben:

![SQLInjection](https://user-images.githubusercontent.com/69577267/206466005-69d62477-c6c9-4b48-a467-58b120dd105b.png)


## Verifikation

Ich habe anhand der Informationen aus dem Auftrag, der Inputs aus dem Unterricht von Herrn Colic und seine zur Verfügung gestellten Präsentationen gelernt, wie man eine SQL-Interpreter Injection auf einer Webseite abfangen kann anhand von preparedStatements.


# Reflektion zum Arbeitsprozess

👍  
Die Arbeit an diesem Auftrag lief relativ gut ab. Ich habe schon davor mit PreparedStatements an einem JavaFx gearbeitet und war mir deswegen ihrer Verwendungsweise schon bewusst. Der Refresher durch die zwei vorgegebenen Links aus dem Auftrag waren jedoch stark notwendig.

👎  
Was mir zu Beginn des Auftrages etwas unklar war, ist wie und wo ich das PreparedStatement einsetzen musste. Dies hat sich jedoch schnell gelöst. Unter anderem hat mich der Datentyp TimeStap etwas verwirrt und ich wusste nicht ganz, wie ich daraus ein PreparedStatement machen kann. Dies hat sich auch flott gelöst durch eine schnelle Googlerecherche.

**VBV**: ✍️ Ich möchte aktiver am Unterricht vor Ort teilnehmen, um so noch mehr Wissen aufzubauen. Um diese Verbesserung umzusetzen werde ich versuchen während eines Erklärungsblockes meinen Laptop zu schliessen oder jegliche unnötigen Ablenkungen zu eliminieren, um mich so besser zu fokussieren.
