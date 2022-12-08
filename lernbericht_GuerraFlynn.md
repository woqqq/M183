# Lern-Bericht
von Flynn Guerra

## Einleitung

In diesem gesamtem Modul ging es darum, wie man eine Webseite sicher gestalten kann um die Sicherheitslücken so gering wie möglich zu behalten.
In diesem Lernbericht geht es darum wie man eine Webseite vor einer SQL-Injection schützt.

## Was habe ich gelernt?

Ich habe in diesem Projekt gelernt wie man eine SQL-Interpreter Injection anhand von preparedStatements abfängt und so eine Webseite davor sichert.

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

Die Sicherheitslücke in diesem

## Verifikation

✍️ Erklären Sie kurz und bündig, inwiefern die von Ihnen verwendeten Medien zeigen, was Sie gelernt haben.

# Reflektion zum Arbeitsprozess

👍 Überlegen Sie sich jeweils etwas, was gut an Ihrer Arbeit lief; 

👎 und etwas, was nicht gut lief.

**VBV**: ✍️ Formulieren Sie davon ausgehend einen *handelbaren* Verbesserungsvorschlag.
