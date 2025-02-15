---
title: 'Tutorial: Azure Active Directory-Integration mit Syncplicity | Microsoft Docs'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und Syncplicity konfigurieren.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 06/10/2019
ms.author: jeedes
ms.openlocfilehash: 3c665795325ed3863583eb0f21f3e0d3f534154a
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "103201496"
---
# <a name="tutorial-integrate-syncplicity-with-azure-active-directory"></a>Tutorial: Integrieren von Syncplicity in Azure Active Directory

In diesem Tutorial erfahren Sie, wie Sie Syncplicity in Azure Active Directory (Azure AD) integrieren. Bei der Integration von Syncplicity in Azure AD haben Sie folgende Möglichkeiten:

* Sie können in Azure AD steuern, wer Zugriff auf Syncplicity haben soll.
* Ermöglichen Sie es Ihren Benutzern, sich mit ihren Azure AD-Konten automatisch bei Syncplicity anzumelden.
* Verwalten Sie Ihre Konten zentral im Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

## <a name="prerequisites"></a>Voraussetzungen

Für die ersten Schritte benötigen Sie Folgendes:

* Ein Azure AD-Abonnement Falls Sie kein Abonnement besitzen, können Sie [hier](https://azure.microsoft.com/free/) eine zwölfmonatige kostenlose Testversion erhalten.
* Ein Syncplicity-Abonnement, für das einmaliges Anmelden aktiviert ist

## <a name="scenario-description"></a>Beschreibung des Szenarios

In diesem Tutorial konfigurieren und testen Sie das einmalige Anmelden von Azure AD in einer Testumgebung. Syncplicity unterstützt **SP-initiiertes** einmaliges Anmelden.

## <a name="adding-syncplicity-from-the-gallery"></a>Hinzufügen von Syncplicity über den Katalog

Zum Konfigurieren der Integration von Syncplicity in Azure AD müssen Sie Syncplicity über den Katalog zur Liste der verwalteten SaaS-Apps hinzufügen.

1. Melden Sie sich mit einem Geschäfts-, Schul- oder Unikonto oder mit einem persönlichen Microsoft-Konto beim [Azure-Portal](https://portal.azure.com) an.
1. Wählen Sie im linken Navigationsbereich den Dienst **Azure Active Directory** aus.
1. Klicken Sie unter **Erstellen** auf **Unternehmensanwendung**.
1. Geben Sie im Abschnitt **Azure AD-Katalog durchsuchen** den Suchbegriff **Syncplicity** in das Suchfeld ein.
1. Wählen Sie im Ergebnisbereich **Syncplicity** aus, und klicken Sie dann auf **Erstellen**, um die App hinzuzufügen. Warten Sie einige Sekunden, während die App Ihrem Mandanten hinzugefügt wird.

## <a name="configure-and-test-azure-ad-sso"></a>Konfigurieren und Testen des einmaligen Anmeldens (Single Sign-On, SSO) von Azure AD

Konfigurieren und testen Sie das einmalige Anmelden von Azure AD mit Syncplicity mithilfe eines Testbenutzers mit dem Namen **B. Simon**. Damit einmaliges Anmelden funktioniert, muss eine Linkbeziehung zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in Syncplicity eingerichtet werden.

Zum Konfigurieren und Testen des einmaligen Anmeldens von Azure AD bei Syncplicity müssen Sie die folgenden Bausteine ausführen:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configure-azure-ad-sso)** , um Ihren Benutzern die Verwendung dieses Features zu ermöglichen.
2. **[Konfigurieren des einmaligen Anmeldens für Syncplicity](#configure-syncplicity-sso)**, um die Einstellungen für einmaliges Anmelden auf der Anwendungsseite zu konfigurieren
3. **[Erstellen eines Azure AD-Testbenutzers](#create-an-azure-ad-test-user)** , um das einmalige Anmelden von Azure AD mit dem Testbenutzer B. Simon zu testen.
4. **[Zuweisen des Azure AD-Testbenutzers](#assign-the-azure-ad-test-user)** , um B. Simon die Verwendung des einmaligen Anmeldens von Azure AD zu ermöglichen.
5. **[Erstellen eines Syncplicity-Testbenutzers](#create-syncplicity-test-user)**, um eine Entsprechung von B. Simon in Syncplicity zu erhalten, die mit ihrer Darstellung in Azure AD verknüpft ist
6. **[Testen des einmaligen Anmeldens](#test-sso)** , um zu überprüfen, ob die Konfiguration funktioniert
7. **[Aktualisieren des einmaligen Anmeldens](#update-sso)** , um die erforderlichen Änderungen in Syncplicity vorzunehmen, falls Sie die SSO-Einstellungen in Azure AD geändert haben

### <a name="configure-azure-ad-sso"></a>Konfigurieren des einmaligen Anmeldens (Single Sign-On, SSO) von Azure AD

Gehen Sie wie folgt vor, um das einmalige Anmelden von Azure AD im Azure-Portal zu aktivieren.

1. Navigieren Sie im [Azure-Portal](https://portal.azure.com/) auf der Anwendungsintegrationsseite für **Syncplicity** zum Abschnitt **Erste Schritte**, und wählen Sie **Einmaliges Anmelden einrichten** aus.
2. Wählen Sie auf der Seite **SSO-Methode auswählen** die Methode **SAML** aus.
3. Klicken Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** auf das Bearbeitungs- bzw. Stiftsymbol für **Grundlegende SAML-Konfiguration**, um die Einstellungen zu bearbeiten.

   ![Bearbeiten der SAML-Basiskonfiguration](common/edit-urls.png)

4. Geben Sie im Abschnitt **Grundlegende SAML-Konfiguration** die Werte für die folgenden Felder ein:

    a. Geben Sie im Textfeld **Bezeichner (Entitäts-ID)** eine URL im folgenden Format ein: `https://<companyname>.syncplicity.com/sp`.

    b. Geben Sie im Textfeld **Anmelde-URL** eine URL im folgenden Format ein: `https://<companyname>.syncplicity.com`.
    
    c. Geben Sie im Textfeld **Antwort-URL (Assertionsverbraucherdienst-URL)** eine URL in folgendem Format ein: `https://<companyname>.syncplicity.com/Auth/AssertionConsumerService.aspx`.

    > [!NOTE]
    > Hierbei handelt es sich um Beispielwerte. Ersetzen Sie diese Werte durch die tatsächliche Anmelde-URL und den tatsächlichen Bezeichner. Wenden Sie sich an den [Syncplicity-Support](https://www.syncplicity.com/contact-us), um diese Werte zu erhalten. Sie können sich auch die Muster im Abschnitt **Grundlegende SAML-Konfiguration** im Azure-Portal ansehen.

5. Klicken Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** im Abschnitt **SAML-Signaturzertifikat** auf **Bearbeiten**. Klicken Sie dann im Dialogfeld neben dem aktiven Zertifikat auf die Schaltfläche mit Auslassungspunkten, und wählen Sie **Download des PEM-Zertifikats** aus.

   ![Downloadlink für das Zertifikat](common/certificatebase64.png)

    > [!NOTE]
    > Sie benötigen das PEM-Zertifikat, da Syncplicity keine Zertifikate im CER-Format akzeptiert.

6. Kopieren Sie im Abschnitt **Syncplicity einrichten** die entsprechenden URLs gemäß Ihren Anforderungen.

   ![Kopieren der Konfiguration-URLs](common/copy-configuration-urls.png)

### <a name="configure-syncplicity-sso"></a>Konfigurieren des einmaligen Anmeldens für Syncplicity

1. Melden Sie sich bei Ihrem **Syncplicity**-Mandanten an.

1. Klicken Sie im Menü oben auf **Admin** (Verwaltung), wählen Sie **Settings** (Einstellungen) aus, und klicken Sie dann auf **Custom domain and single sign-on** (Benutzerdefinierte Domäne und einmaliges Anmelden).

    ![Syncplicity](./media/syncplicity-tutorial/ic769545.png "Syncplicity")

1. Führen Sie auf der Dialogseite **Single Sign-On (SSO)** (Einmaliges Anmelden) die folgenden Schritte aus:

    ![Einmaliges Anmelden \(Single Sign-On, SSO\)](./media/syncplicity-tutorial/ic769550.png "Single Sign-On \\\(SSO\\\)")

    a. Geben Sie im Textfeld **Custom Domain** (Benutzerdefinierte Domäne) den Namen Ihrer Domäne ein.
  
    b. Wählen Sie **Enabled** (Aktiviert) für **Single Sign-On Status** (Status der einmaligen Anmeldung) aus.

    c. Fügen Sie in das Textfeld **Entitäts-ID** den Wert von **Bezeichner (Entitäts-ID)** ein, den Sie im Azure-Portal in **Grundlegende SAML-Konfiguration** verwendet haben.

    d. Fügen Sie in das Textfeld **Sign-in page URL** (URL für Anmeldeseite) die **Anmelde-URL** ein, die Sie aus dem Azure-Portal kopiert haben.

    e. Fügen Sie in das Textfeld **Logout page URL** (URL der Abmeldeseite) den Wert der **Abmelde-URL** ein, den Sie aus dem Azure-Portal kopiert haben.

    f. Klicken Sie unter **Zertifikat des Identitätsanbieters** auf **Datei auswählen**, und laden Sie dann das Zertifikat hoch, das Sie aus dem Azure-Portal heruntergeladen haben.

    g. Klicken Sie auf **ÄNDERUNGEN SPEICHERN**.

### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers

In diesem Abschnitt erstellen Sie im Azure-Portal einen Testbenutzer mit dem Namen B. Simon.

1. Wählen Sie im linken Bereich des Microsoft Azure-Portals **Azure Active Directory** > **Benutzer** > **Alle Benutzer** aus.
2. Wählen Sie oben im Bildschirm die Option **Neuer Benutzer** aus.
3. Führen Sie unter den Eigenschaften für **Benutzer** die folgenden Schritte aus:

   a. Geben Sie im Feld **Benutzername** die Zeichenfolge username@companydomain.extension ein. Beispiel: `B.Simon@contoso.com`.

   b. Geben Sie im Feld **Name** die Zeichenfolge `B.Simon` ein.  
   
   c. Aktivieren Sie das Kontrollkästchen **Kennwort anzeigen**, und notieren Sie sich den Wert aus dem Feld **Kennwort**.
   
   d. Klicken Sie auf **Erstellen**.

### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie B. Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf Syncplicity gewähren.

1. Wählen Sie im Azure-Portal **Unternehmensanwendungen** > **Alle Anwendungen** aus.
1. Wählen Sie in der Anwendungsliste **Syncplicity** aus.
1. Navigieren Sie auf der Übersichtsseite der App zum Abschnitt **Verwalten**, und wählen Sie **Benutzer und Gruppen** aus.

   ![Link „Benutzer und Gruppen“](common/users-groups-blade.png)

1. Wählen Sie **Benutzer/Gruppe hinzufügen** aus.

    ![Link „Benutzer hinzufügen“](common/add-assign-user.png)
1. Wählen Sie auf der Seite **Zuweisung hinzufügen** die Option **Benutzer** aus. 
1. Wählen Sie im Dialogfeld **Benutzer** in der Liste „Benutzer“ den Eintrag **B. Simon** aus, und klicken Sie dann unten auf dem Bildschirm auf die Schaltfläche **Auswählen**.
1. Wenn Sie einen beliebigen Rollenwert in der SAML-Assertion erwarten, wählen Sie im Dialogfeld **Rolle auswählen** die entsprechende Rolle für den Benutzer in der Liste aus, und klicken Sie dann im unteren Bildschirmbereich auf die Schaltfläche **Auswählen**.
1. Klicken Sie auf der Seite **Zuweisung hinzufügen** auf die Schaltfläche **Zuweisen**.

### <a name="create-syncplicity-test-user"></a>Erstellen eines Testbenutzers für Syncplicity

Damit sich Azure AD-Benutzer anmelden können, müssen sie in der Syncplicity-Anwendung bereitgestellt werden. In diesem Abschnitt wird beschrieben, wie in Syncplicity Azure AD-Benutzerkonten erstellt werden.

**Führen Sie zum Bereitstellen von Benutzerkonten in Syncplicity die folgenden Schritte aus:**

1. Melden Sie sich bei Ihrem **Syncplicity** -Mandanten an (Beispiel: `https://company.Syncplicity.com`).

1. Klicken Sie auf **Admin** (Verwaltung), wählen Sie **User Accounts** (Benutzerkonten) aus, und klicken Sie dann auf **Add a user** (Benutzer hinzufügen).

    ![Verwalten von Benutzern](./media/syncplicity-tutorial/ic769764.png "Benutzer verwalten")

1. Geben Sie die **E-Mail-Adresse** eines Azure AD-Kontos ein, das Sie bereitstellen möchten, und wählen Sie dann für **Role** (Rolle) die Option **User** (Benutzer) aus. Klicken Sie abschließend auf **Next** (Weiter).

    ![Azure-Kontoinformationen](./media/syncplicity-tutorial/ic769765.png "Azure-Kontoinformationen")

    > [!NOTE]
    > Der Azure AD-Kontoinhaber erhält eine E-Mail mit einem Link zur Bestätigung und Aktivierung des Kontos.

1. Wählen Sie eine Gruppe in Ihrem Unternehmen aus, zu dem der neue Benutzer gehören soll, und klicken Sie dann auf **Next** (Weiter).

    ![Gruppenmitgliedschaft](./media/syncplicity-tutorial/ic769772.png "Gruppenmitgliedschaft")

    > [!NOTE]
    > Wenn keine Gruppen aufgelistet sind, klicken Sie einfach auf **Next** (Weiter).

1. Wählen Sie die Ordner aus, die Sie auf dem Computer des Benutzers von Syncplicity steuern lassen möchten, und klicken Sie dann auf **Next** (Weiter).

    ![Syncplicity-Ordner](./media/syncplicity-tutorial/ic769773.png "Syncplicity-Ordner")

> [!NOTE]
> Sie können Azure AD-Benutzerkonten auch mithilfe anderer Tools zum Erstellen von Syncplicity-Benutzerkonten oder mithilfe der von Syncplicity bereitgestellten APIs erstellen.

### <a name="test-sso"></a>Testen des einmaligen Anmeldens

Wenn Sie im Zugriffsbereich auf die Kachel „Syncplicity“ klicken, sollten Sie automatisch bei der Syncplicity-Instanz angemeldet werden, für die Sie einmaliges Anmelden eingerichtet haben. Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](../user-help/my-apps-portal-end-user-access.md).

### <a name="update-sso"></a>Aktualisieren des einmaligen Anmeldens

Wenn Sie Änderungen am einmaligen Anmelden vornehmen müssen, müssen Sie das verwendete **SAML-Signaturzertifikat** überprüfen. Hat sich das Zertifikat geändert, stellen Sie sicher, dass das neue Zertifikat in Syncplicity hochgeladen wird, wie unter **[Konfigurieren des einmaligen Anmeldens für Syncplicity](#configure-syncplicity-sso)** beschrieben.

Wenden Sie sich bei Verwendung der mobilen Syncplicity-App an den Syncplicity-Kundensupport (support@syncplicity.com), um Unterstützung zu erhalten.

## <a name="additional-resources"></a>Weitere Ressourcen

- [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](./tutorial-list.md)

- [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

- [Was ist bedingter Zugriff?](../conditional-access/overview.md)
