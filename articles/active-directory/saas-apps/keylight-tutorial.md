---
title: 'Tutorial: Azure Active Directory-Integration in LockPath Keylight | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden für Azure Active Directory und LockPath Keylight konfigurieren.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 04/14/2019
ms.author: jeedes
ms.openlocfilehash: 81949736603d22cac779d08d14bd6db65065d730
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "92459083"
---
# <a name="tutorial-azure-active-directory-integration-with-lockpath-keylight"></a>Tutorial: Azure Active Directory-Integration in LockPath Keylight

In diesem Tutorial erfahren Sie, wie Sie LockPath Keylight in Azure Active Directory (Azure AD) integrieren.
Die Integration von LockPath Keylight in Azure AD bietet die folgenden Vorteile:

* Sie können in Azure AD steuern, wer Zugriff auf LockPath Keylight hat.
* Sie können es Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei LockPath Keylight anzumelden (einmaliges Anmelden; Single Sign-On, SSO).
* Sie können Ihre Konten über das Azure-Portal an einem zentralen Ort verwalten.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).
Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration mit LockPath Keylight konfigurieren zu können, benötigen Sie Folgendes:

* Ein Azure AD-Abonnement Sollten Sie über keine Azure AD-Umgebung verfügen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) verwenden.
* LockPath Keylight-Abonnement, für das einmaliges Anmelden aktiviert ist

## <a name="scenario-description"></a>Beschreibung des Szenarios

In diesem Tutorial konfigurieren und testen Sie das einmalige Anmelden von Azure AD in einer Testumgebung.

* LockPath Keylight unterstützt **SP-initiiertes** einmaliges Anmelden.
* LockPath Keylight unterstützt die **Just-in-Time**-Benutzerbereitstellung.

## <a name="adding-lockpath-keylight-from-the-gallery"></a>Hinzufügen von LockPath Keylight aus dem Katalog

Zum Konfigurieren der Integration von LockPath Keylight in Azure AD müssen Sie LockPath Keylight aus dem Katalog zur Liste mit den verwalteten SaaS-Apps hinzufügen.

**Um LockPath Keylight aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**.

    ![Schaltfläche „Azure Active Directory“](common/select-azuread.png)

2. Navigieren Sie zu **Unternehmensanwendungen**, und wählen Sie die Option **Alle Anwendungen** aus.

    ![Blatt „Unternehmensanwendungen“](common/enterprise-applications.png)

3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![Schaltfläche „Neue Anwendung“](common/add-new-app.png)

4. Geben Sie im Suchfeld **LockPath Keylight** ein, wählen Sie im Ergebnisbereich **LockPath Keylight** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![LockPath Keylight in der Ergebnisliste](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurieren und Testen des einmaligen Anmeldens in Azure AD

In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden von Azure AD mit LockPath Keylight mithilfe eines Testbenutzers namens **Britta Simon**.
Damit einmaliges Anmelden funktioniert, muss eine Linkbeziehung zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in LockPath Keylight eingerichtet werden.

Zum Konfigurieren und Testen des einmaligen Anmeldens in Azure AD mit LockPath Keylight müssen Sie die folgenden Schritte ausführen:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configure-azure-ad-single-sign-on)** , um Ihren Benutzern das Verwenden dieses Features zu ermöglichen.
2. **[Konfigurieren des einmaligen Anmeldens für LockPath Keylight](#configure-lockpath-keylight-single-sign-on)** , um die Einstellungen für einmaliges Anmelden auf der Anwendungsseite zu konfigurieren
3. **[Erstellen eines Azure AD-Testbenutzers](#create-an-azure-ad-test-user)** , um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
4. **[Zuweisen des Azure AD-Testbenutzers](#assign-the-azure-ad-test-user)** , um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Erstellen eines LockPath Keylight-Testbenutzers](#create-lockpath-keylight-test-user)** , um ein Pendant zu Britta Simon in LockPath Keylight zu erhalten, das mit ihrer Darstellung in Azure AD verknüpft ist
6. **[Testen der einmaligen Anmeldung](#test-single-sign-on)** , um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens in Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden von Azure AD im Azure-Portal.

Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD mit LockPath Keylight die folgenden Schritte aus:

1. Wählen Sie im [Azure-Portal](https://portal.azure.com/) auf der Anwendungsintegrationsseite für **LockPath Keylight** die Option **Einmaliges Anmelden** aus.

    ![Konfigurieren des Links für einmaliges Anmelden](common/select-sso.png)

2. Wählen Sie im Dialogfeld **SSO-Methode auswählen** den Modus **SAML/WS-Fed** aus, um einmaliges Anmelden zu aktivieren.

    ![Auswahlmodus für einmaliges Anmelden](common/select-saml-option.png)

3. Klicken Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** auf das Symbol **Bearbeiten**, um das Dialogfeld **Grundlegende SAML-Konfiguration** zu öffnen.

    ![Bearbeiten der SAML-Basiskonfiguration](common/edit-urls.png)

4. Führen Sie im Abschnitt **Grundlegende SAML-Konfiguration** die folgenden Schritte aus:

    ![SSO-Informationen zur Domäne und zu den URLs für LockPath Keylight](common/sp-identifier-reply.png)

    a. Geben Sie im Textfeld **Anmelde-URL** eine URL im folgenden Format ein: `https://<company name>.keylightgrc.com/`.

    b. Geben Sie im Textfeld **Bezeichner (Entitäts-ID)** eine URL im folgenden Format ein: `https://<company name>.keylightgrc.com`.

    c. Geben Sie im Textfeld **Antwort-URL** eine URL nach folgendem Muster ein: `https://<company name>.keylightgrc.com/Login.aspx`

    > [!NOTE]
    > Hierbei handelt es sich um Beispielwerte. Ersetzen Sie diese Werte durch die tatsächlichen Werte für die Anmelde-URL, den Bezeichner und die Antwort-URL. Wenden Sie sich an das [Supportteam für den LockPath Keylight-Client](https://www.lockpath.com/contact/), um diese Werte zu erhalten. Sie können sich auch die Muster im Abschnitt **Grundlegende SAML-Konfiguration** im Azure-Portal ansehen.

5. Klicken Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** im Abschnitt **SAML-Signaturzertifikat** auf **Herunterladen**, um das Ihrer Anforderung entsprechende Zertifikat (**Zertifikat (Rohdaten)** ) aus den angegebenen Optionen herunterzuladen und auf Ihrem Computer zu speichern.

    ![Downloadlink für das Zertifikat](common/certificateraw.png)

6. Kopieren Sie im Abschnitt **LockPath Keylight einrichten** die entsprechenden URLs gemäß Ihren Anforderungen.

    ![Kopieren der Konfiguration-URLs](common/copy-configuration-urls.png)

    a. Anmelde-URL

    b. Azure AD-Bezeichner

    c. Abmelde-URL

### <a name="configure-lockpath-keylight-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens für LockPath Keylight

1. Führen Sie die folgenden Schritte aus, um das einmalige Anmelden in LockPath Keylight zu aktivieren:

    a. Melden Sie sich als Administrator bei Ihrem LockPath Keylight-Konto an.

    b. Klicken Sie im Menü oben auf **Person**, und wählen Sie **Keylight Setup** aus.

    ![Screenshot: Auswahl des Symbols „Person“ und der Option „Keylight Setup“ im Dropdownmenü](./media/keylight-tutorial/401.png)

    c. Klicken Sie in der Strukturansicht links auf **SAML**.

    ![Screenshot: Auswahl von „SAML“ in der Strukturansicht](./media/keylight-tutorial/402.png)

    d. Klicken Sie im Dialogfeld **SAML-Einstellungen** auf **Bearbeiten**.

    ![Screenshot: Fenster „SAML-Einstellungen“ mit Auswahl der Schaltfläche „Bearbeiten“](./media/keylight-tutorial/404.png)

1. Führen Sie auf der Dialogfeldseite **Edit SAML Settings** die folgenden Schritte aus:

    ![Einmaliges Anmelden konfigurieren](./media/keylight-tutorial/405.png)

    a. Legen Sie für **SAML-Authentifizierung** die Einstellung **Aktiv** fest.

    b. Fügen Sie in das Textfeld **Identity Provider Login URL** (Anmelde-URL des Identitätsanbieters) den Wert der **Anmelde-URL** ein, den Sie aus dem Azure-Portal kopiert haben.

    c. Fügen Sie in das Textfeld **Identity Provider Logout URL** (Abmelde-URL des Identitätsanbieters) den Wert der **Abmelde-URL** ein, den Sie aus dem Azure-Portal kopiert haben.

    d. Klicken Sie auf **Datei auswählen**, um das heruntergeladene LockPath Keylight-Zertifikat auszuwählen, und klicken Sie dann auf **Öffnen**, um das Zertifikat hochzuladen.

    e. Legen Sie für **Speicherort der SAML-Benutzer-ID** die Einstellung **NameIdentifier-Element der subject-Anweisung** fest.

    f. Geben Sie den **Keylight-Dienstanbieter** in folgendem Format ein: `https://<CompanyName>.keylightgrc.com`.

    g. Legen Sie für **Benutzer automatisch bereitstellen** die Einstellung **Aktiv** fest.

    h. Legen Sie für **Kontotyp automatisch bereitstellen** die Einstellung **Vollständiger Benutzer** fest.

    i. Wählen Sie für **Sicherheitsrolle automatisch bereitstellen** die Option **Standardbenutzer mit SAML** aus.

    j. Legen Sie für **Sicherheitskonfiguration automatisch bereitstellen** die Einstellung **Standardbenutzerkonfiguration** fest.

    k. Geben Sie im Textfeld **E-Mail-Attribut** die Zeichenfolge `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress` ein.

    l. Geben Sie **in das Textfeld** Vornamen-Attribut`http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname` ein.

    m. Geben Sie **in das Textfeld** Nachnamen-Attribut`http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname` ein.

    n. Klicken Sie auf **Speichern**.

### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers

Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im Azure-Portal.

1. Wählen Sie im Azure-Portal im linken Bereich die Option **Azure Active Directory**, **Benutzer** und dann **Alle Benutzer** aus.

    ![Links „Benutzer und Gruppen“ und „Alle Benutzer“](common/users.png)

2. Wählen Sie oben im Bildschirm die Option **Neuer Benutzer** aus.

    ![Schaltfläche „Neuer Benutzer“](common/new-user.png)

3. Führen Sie in den Benutzereigenschaften die folgenden Schritte aus.

    ![Dialogfeld „Benutzer“](common/user-properties.png)

    a. Geben Sie im Feld **Name** den Namen **BrittaSimon** ein.
  
    b. Geben Sie im Feld **Benutzername** den Namen `brittasimon@yourcompanydomain.extension` ein. Zum Beispiel, BrittaSimon@contoso.com

    c. Aktivieren Sie das Kontrollkästchen **Kennwort anzeigen**, und notieren Sie sich den Wert, der im Feld „Kennwort“ angezeigt wird.

    d. Klicken Sie auf **Erstellen**.

### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf LockPath Keylight gewähren.

1. Wählen Sie im Azure-Portal **Unternehmensanwendungen** > **Alle Anwendungen** > **LockPath Keylight** aus.

    ![Blatt „Unternehmensanwendungen“](common/enterprise-applications.png)

2. Wählen Sie in der Anwendungsliste **LockPath Keylight** aus.

    ![LockPath Keylight-Link in der Anwendungsliste](common/all-applications.png)

3. Wählen Sie im Menü auf der linken Seite **Benutzer und Gruppen** aus.

    ![Link „Benutzer und Gruppen“](common/users-groups-blade.png)

4. Klicken Sie auf die Schaltfläche **Benutzer hinzufügen**, und wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Bereich „Zuweisung hinzufügen“](common/add-assign-user.png)

5. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Liste „Benutzer“ den Eintrag **Britta Simon** aus, und klicken Sie dann unten im Bildschirm auf die Schaltfläche **Auswählen**.

6. Wenn Sie einen beliebigen Rollenwert in der SAML-Assertion erwarten, wählen Sie im Dialogfeld **Rolle auswählen** in der Liste die entsprechende Rolle für den Benutzer aus, und klicken Sie dann unten auf dem Bildschirm auf **Auswählen**.

7. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf die Schaltfläche **Zuweisen**.

### <a name="create-lockpath-keylight-test-user"></a>Erstellen eines LockPath Keylight-Testbenutzers

In diesem Abschnitt wird in LockPath Keylight ein Benutzer mit dem Namen Britta Simon erstellt. LockPath Keylight unterstützt die Just-in-Time-Bereitstellung, die standardmäßig aktiviert ist. Für Sie steht in diesem Abschnitt kein Aktionselement zur Verfügung. Ist ein Benutzer noch nicht in LockPath Keylight vorhanden, wird nach der Authentifizierung ein neuer Benutzer erstellt. Wenn Sie einen Benutzer manuell erstellen müssen, setzen Sie sich mit dem [Supportteam für den LockPath Keylight-Client](https://www.lockpath.com/contact/) in Verbindung.

### <a name="test-single-sign-on"></a>Testen des einmaligen Anmeldens

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „LockPath Keylight“ klicken, sollten Sie automatisch bei der LockPath Keylight-Instanz angemeldet werden, für die Sie einmaliges Anmelden eingerichtet haben. Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](../user-help/my-apps-portal-end-user-access.md).

## <a name="additional-resources"></a>Weitere Ressourcen

- [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](./tutorial-list.md)

- [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

- [Was ist bedingter Zugriff?](../conditional-access/overview.md)