---
title: Erweiterte Verwendung von AuthN/AuthZ
description: Erfahren Sie, wie Sie das Feature zur Authentifizierung und Autorisierung in App Service für unterschiedliche Szenarien verwenden und Benutzeransprüche und verschiedene Token abrufen.
ms.topic: article
ms.date: 03/29/2021
ms.custom: seodec18, devx-track-azurecli
ms.openlocfilehash: 7209f5176480577f14ba5b02fafc5a249c58fd1c
ms.sourcegitcommit: 80d311abffb2d9a457333bcca898dfae830ea1b4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/25/2021
ms.locfileid: "110452220"
---
# <a name="advanced-usage-of-authentication-and-authorization-in-azure-app-service"></a>Erweiterte Verwendung der Authentifizierung und Autorisierung in Azure App Service

Dieser Artikel zeigt, wie Sie die integrierte [Authentifizierung und Autorisierung in App Service](overview-authentication-authorization.md) anpassen und Identitäten über Ihre Anwendung verwalten. 

Sehen Sie sich eines der folgenden Tutorials an, um sofort loszulegen:

* [Tutorial: Umfassendes Authentifizieren und Autorisieren von Benutzern in Azure App Service](tutorial-auth-aad.md)
* [Konfigurieren Ihrer App zur Verwendung der Microsoft Identity Platform-Anmeldung](configure-authentication-provider-aad.md)
* [Konfigurieren Ihrer App Service-Anwendung zur Verwendung der Facebook-Anmeldung](configure-authentication-provider-facebook.md)
* [Konfigurieren Ihrer App Service-Anwendung zur Verwendung der Google-Anmeldung](configure-authentication-provider-google.md)
* [Konfigurieren Ihrer App Service-Anwendung zur Verwendung der Twitter-Anmeldung](configure-authentication-provider-twitter.md)
* [Konfigurieren Ihrer App für die Anmeldung über einen OpenID Connect-Anbieter (Vorschau)](configure-authentication-provider-openid-connect.md)
* [Konfigurieren Ihrer App für die Anmeldung mithilfe einer Apple-Anmeldung (Vorschau)](configure-authentication-provider-apple.md)

## <a name="use-multiple-sign-in-providers"></a>Verwenden mehrerer Anmeldungsanbieter

Die Portalkonfiguration bietet keine einfache Möglichkeit, Ihren Benutzern mehrere Anmeldungsanbieter (z.B. sowohl Facebook als auch Twitter) bereitzustellen. Allerdings lässt sich diese Funktionalität Ihrer App problemlos hinzufügen. Dazu sind folgende Schritte erforderlich:

Zunächst konfigurieren Sie im Azure-Portal auf der Seite **Authentifizierung/Autorisierung** alle Identitätsanbieter, die Sie aktivieren möchten.

Wählen Sie für **Die auszuführende Aktion, wenn die Anforderung nicht authentifiziert ist** die Option **Anonyme Anforderungen zulassen (keine Aktion)** aus.

Fügen Sie auf der Anmeldeseite, der Navigationsleiste oder an einer anderen Stelle in Ihrer App einen Anmeldelink für alle Anbieter hinzu, die Sie aktiviert haben (`/.auth/login/<provider>`). Beispiel:

```html
<a href="/.auth/login/aad">Log in with the Microsoft Identity Platform</a>
<a href="/.auth/login/facebook">Log in with Facebook</a>
<a href="/.auth/login/google">Log in with Google</a>
<a href="/.auth/login/twitter">Log in with Twitter</a>
<a href="/.auth/login/apple">Log in with Apple</a>
```

Wenn der Benutzer auf einen der Links klickt, wird die entsprechende Anmeldeseite geöffnet, um den Benutzer anzumelden.

Um den Benutzer nach der Anmeldung auf eine benutzerdefinierte URL umzuleiten, verwenden Sie den `post_login_redirect_url`-Abfragezeichenfolgen-Parameter (nicht zu verwechseln mit der Umleitungs-URI in der Konfiguration Ihres Identitätsanbieters). Zur Umleitung des Benutzers auf `/Home/Index` nach der Anmeldung verwenden Sie zum Beispiel den folgenden HTML-Code:

```html
<a href="/.auth/login/<provider>?post_login_redirect_url=/Home/Index">Log in</a>
```

## <a name="validate-tokens-from-providers"></a>Überprüfen von Token von einem Anbieter

In einer clientgeführten Anmeldung meldet die Anwendung den Benutzer manuell beim Anbieter an und sendet dann das Authentifizierungstoken zur Überprüfung an App Service. (Informationen hierzu finden Sie unter [Authentifizierungsflow](overview-authentication-authorization.md#authentication-flow).) Diese Überprüfung allein gewährt Ihnen noch keinen Zugriff auf die gewünschten App-Ressourcen. Bei erfolgreicher Überprüfung erhalten Sie jedoch ein Sitzungstoken, das Sie für den Zugriff auf App-Ressourcen verwenden können. 

Um das Anbietertoken zu überprüfen, muss die App Service-App zunächst mit dem gewünschten Anbieter konfiguriert werden. Zur Laufzeit, nachdem Sie das Authentifizierungstoken von Ihrem Anbieter abgerufen haben, posten Sie das Token zur Überprüfung unter `/.auth/login/<provider>`. Beispiel: 

```
POST https://<appname>.azurewebsites.net/.auth/login/aad HTTP/1.1
Content-Type: application/json

{"id_token":"<token>","access_token":"<token>"}
```

Das Tokenformat kann je nach Anbieter leicht variieren. Details finden Sie in der folgenden Tabelle:

| Anbieterwert | Im Anforderungstext erforderlich | Kommentare |
|-|-|-|
| `aad` | `{"access_token":"<access_token>"}` | |
| `microsoftaccount` | `{"access_token":"<token>"}` | Die `expires_in`-Eigenschaft ist optional. <br/>Fordern Sie beim Anfordern des Tokens von Livediensten immer den Bereich `wl.basic` an. |
| `google` | `{"id_token":"<id_token>"}` | Die `authorization_code`-Eigenschaft ist optional. Wenn sie angegeben wird, kann sie optional auch zusammen mit der Eigenschaft `redirect_uri` verwendet werden. |
| `facebook`| `{"access_token":"<user_access_token>"}` | Verwenden Sie ein gültiges [Benutzerzugriffstoken](https://developers.facebook.com/docs/facebook-login/access-tokens) aus Facebook. |
| `twitter` | `{"access_token":"<access_token>", "access_token_secret":"<acces_token_secret>"}` | |
| | | |

Wenn das Anbietertoken erfolgreich überprüft wird, gibt die API im Antworttext ein `authenticationToken` zurück. Dies ist Ihr Sitzungstoken. 

```json
{
    "authenticationToken": "...",
    "user": {
        "userId": "sid:..."
    }
}
```

Sobald Sie dieses Sitzungstoken erhalten haben, können Sie auf geschützte App-Ressourcen zugreifen, indem Sie Ihren HTTP-Anforderungen den Header `X-ZUMO-AUTH` hinzufügen. Beispiel: 

```
GET https://<appname>.azurewebsites.net/api/products/1
X-ZUMO-AUTH: <authenticationToken_value>
```

## <a name="sign-out-of-a-session"></a>Abmelden von einer Sitzung

Benutzer können eine Abmeldung initiieren, indem Sie eine `GET`-Anforderung an den Endpunkt `/.auth/logout` der App senden. Die `GET`-Anforderung bewirkt Folgendes:

- Löscht Authentifizierungscookies aus der aktuellen Sitzung
- Löscht die Token des aktuellen Benutzers aus dem Tokenspeicher
- Führt für Azure Active Directory und Google eine serverseitige Abmeldung für den Identitätsanbieter aus

Hier ist ein einfacher Abmeldungslink auf einer Webseite:

```html
<a href="/.auth/logout">Sign out</a>
```

Standardmäßig wird bei einer erfolgreichen Abmeldung der Client an die URL `/.auth/logout/done` weitergeleitet. Sie können die Weiterleitungsseite nach der Abmeldung ändern, indem Sie den Abfrageparameter `post_logout_redirect_uri` hinzufügen. Beispiel:

```
GET /.auth/logout?post_logout_redirect_uri=/index.html
```

Es wird empfohlen, den Wert von `post_logout_redirect_uri` zu [codieren](https://wikipedia.org/wiki/Percent-encoding).

Wenn Sie vollqualifizierte URLs verwenden, muss die URL in derselben Domäne gehostet oder als zulässige externe Weiterleitungs-URL für Ihre App konfiguriert werden. Im folgenden Beispiel erfolgt einer Weiterleitung an die URL `https://myexternalurl.com`, die nicht in derselben Domäne gehostet wird:

```
GET /.auth/logout?post_logout_redirect_uri=https%3A%2F%2Fmyexternalurl.com
```

Führen Sie den nachstehenden Befehl in der [Azure Cloud Shell](../cloud-shell/quickstart.md) aus:

```azurecli-interactive
az webapp auth update --name <app_name> --resource-group <group_name> --allowed-external-redirect-urls "https://myexternalurl.com"
```

## <a name="preserve-url-fragments"></a>Beibehalten von URL-Fragmenten

Nachdem sich Benutzer bei Ihrer App anmelden, möchten sie in der Regel zum selben Abschnitt aus derselben Seite weitergeleitet werden, z.B. `/wiki/Main_Page#SectionZ`. Da [URL-Fragmente](https://wikipedia.org/wiki/Fragment_identifier) (etwa `#SectionZ`) jedoch nie an den Server gesendet werden, werden sie standardmäßig nach Abschluss der OAuth-Anmeldung und der Weiterleitung zurück zu Ihrer App nicht beibehalten. Benutzer erhalten dadurch ein suboptimales Erlebnis, wenn sie wieder zum gewünschten Anker navigieren müssen. Diese Einschränkung betrifft alle serverseitigen Authentifizierungslösungen.

Bei der App Service-Authentifizierung können Sie die URL-Fragmente während der OAuth-Anmeldung beibehalten. Legen Sie dazu die App-Einstellung `WEBSITE_AUTH_PRESERVE_URL_FRAGMENT` auf `true` fest. Sie können dies im [Azure-Portal](https://portal.azure.com) erledigen oder einfach den folgenden Befehl in der [Azure Cloud Shell](../cloud-shell/quickstart.md) ausführen:

```azurecli-interactive
az webapp config appsettings set --name <app_name> --resource-group <group_name> --settings WEBSITE_AUTH_PRESERVE_URL_FRAGMENT="true"
```

## <a name="access-user-claims"></a>Zugriff auf Benutzeransprüche

App Service übergibt Benutzeransprüche mithilfe spezieller Header an Ihre Anwendung. Externe Anforderungen sind nicht zum Festlegen dieser Header berechtigt, sie sind also nur vorhanden, wenn sie von App Service festgelegt wurden. Beispiele für solche Header wären etwa:

* X-MS-CLIENT-PRINCIPAL-NAME
* X-MS-CLIENT-PRINCIPAL-ID

Anhand dieser Header kann jeder Code unabhängig von Programmiersprache oder Framework die benötigten Informationen ermitteln. Bei ASP.NET 4.6-Apps wird **ClaimsPrincipal** automatisch mit entsprechenden Werten festgelegt. ASP.NET Core hingegen bietet keine Authentifizierungs-Middleware, die mit App Service-Benutzeransprüchen integriert ist. Eine Problemumgehung finden Sie unter [MaximeRouiller.Azure.AppService.EasyAuth](https://github.com/MaximRouiller/MaximeRouiller.Azure.AppService.EasyAuth).

Wenn der [Tokenspeicher](overview-authentication-authorization.md#token-store) für Ihre App aktiviert ist, können Sie auch zusätzliche Details zum authentifizierten Benutzer abrufen, indem Sie `/.auth/me` aufrufen. Die Mobile Apps-Server-SDKs enthalten Hilfsmethoden für die Verwendung dieser Daten. Weitere Informationen finden Sie unter [Verwenden des Azure Mobile Apps SDK für Node.js](/previous-versions/azure/app-service-mobile/app-service-mobile-node-backend-how-to-use-server-sdk#howto-tables-getidentity) sowie unter [Arbeiten mit dem .NET-Back-End-Server-SDK für Azure Mobile Apps](/previous-versions/azure/app-service-mobile/app-service-mobile-dotnet-backend-how-to-use-server-sdk#user-info).

## <a name="retrieve-tokens-in-app-code"></a>Abrufen von Token in App-Code

Die anbieterspezifischen Token werden aus dem Servercode in den Anforderungsheader eingefügt, sodass Sie problemlos darauf zugreifen können. Die folgende Tabelle zeigt mögliche Namen von Tokenheadern:

| Anbieter | Headernamen |
|-|-|
| Azure Active Directory | `X-MS-TOKEN-AAD-ID-TOKEN` <br/> `X-MS-TOKEN-AAD-ACCESS-TOKEN` <br/> `X-MS-TOKEN-AAD-EXPIRES-ON`  <br/> `X-MS-TOKEN-AAD-REFRESH-TOKEN` |
| Facebook-Token | `X-MS-TOKEN-FACEBOOK-ACCESS-TOKEN` <br/> `X-MS-TOKEN-FACEBOOK-EXPIRES-ON` |
| Google | `X-MS-TOKEN-GOOGLE-ID-TOKEN` <br/> `X-MS-TOKEN-GOOGLE-ACCESS-TOKEN` <br/> `X-MS-TOKEN-GOOGLE-EXPIRES-ON` <br/> `X-MS-TOKEN-GOOGLE-REFRESH-TOKEN` |
| Twitter | `X-MS-TOKEN-TWITTER-ACCESS-TOKEN` <br/> `X-MS-TOKEN-TWITTER-ACCESS-TOKEN-SECRET` |
|||

Senden Sie von Ihrem Clientcode (z. B. einer mobilen App oder JavaScript im Browser) eine HTTP-`GET`-Anforderung an `/.auth/me` (der [Tokenspeicher](overview-authentication-authorization.md#token-store) muss aktiviert sein). Der zurückgegebene JSON-Code enthält die anbieterspezifischen-Token.

> [!NOTE]
> Zugriffstoken sind für den Zugriff auf Anbieterressourcen vorgesehen, daher sind sie nur vorhanden, wenn Sie den Anbieter mit einem geheimen Clientschlüssel konfigurieren. Informationen zum Abrufen von Aktualisierungstoken finden Sie unter „Zugriffstoken für die Aktualisierung“.

## <a name="refresh-identity-provider-tokens"></a>Aktualisieren von Identitätsanbieter-Token

Wenn das Zugriffstoken Ihres Anbieters (nicht das [Sitzungstoken](#extend-session-token-expiration-grace-period)) abgelaufen ist, müssen Sie den Benutzer erneut authentifizieren, bevor Sie dieses Token erneut verwenden. Sie können den Tokenablauf vermeiden, indem Sie einen `GET`-Aufruf an den `/.auth/refresh`-Endpunkt Ihrer Anwendung durchführen. Bei einem Aufruf aktualisiert App Service automatisch die Zugriffstoken im [Tokenspeicher](overview-authentication-authorization.md#token-store) für den authentifizierten Benutzer. Bei nachfolgenden Anforderungen von Token durch Ihren App-Code werden die aktualisierten Token abgerufen. Damit die Tokenaktualisierung funktioniert, muss der Tokenspeicher jedoch [Aktualisierungstoken](https://auth0.com/learn/refresh-tokens/) für Ihren Anbieter enthalten. Die jeweilige Methode zum Abrufen von Aktualisierungstoken ist von den einzelnen Anbietern dokumentiert, die folgende Liste stellt jedoch eine kurze Zusammenfassung dar:

- **Google**: Fügen Sie einen Abfragezeichenfolgen-Parameter vom Typ `access_type=offline` an Ihren `/.auth/login/google`-API-Aufruf an. Bei Verwendung des Mobile Apps SDK können Sie den Parameter einer der `LogicAsync`-Überladungen hinzufügen (siehe [Google-Aktualisierungstoken](https://developers.google.com/identity/protocols/OpenIDConnect#refresh-tokens)).
- **Facebook**: Stellt keine Aktualisierungstoken bereit. Langlebige Token laufen nach 60 ab (siehe [Verlängern von Zugriffsschlüsseln für Seiten](https://developers.facebook.com/docs/facebook-login/access-tokens/expiration-and-extension)).
- **Twitter**: Zugriffstoken laufen nicht ab (siehe [Häufig gestellte Fragen zu OAuth für Twitter](https://developer.twitter.com/en/docs/authentication/faq)).
- **Azure Active Directory:** Führen Sie in [https://resources.azure.com](https://resources.azure.com) folgende Schritte aus:
    1. Wählen Sie am oberen Seitenrand die Option **Lesen/Schreiben** aus.
    2. Navigieren Sie im linken Browser zu **subscriptions** > ** _\<subscription\_name_** > **resourceGroups** > **_ \<resource\_group\_name> _** > **providers** > **Microsoft.Web** > **sites** > **_ \<app\_name>_** > **config** > **authsettings**. 
    3. Klicken Sie auf **Bearbeiten**.
    4. Ändern Sie die folgende Eigenschaft. Ersetzen Sie _\<app\_id>_ durch die ID der Azure Active Directory-Anwendung des Diensts, auf den Sie zugreifen möchten.

        ```json
        "additionalLoginParams": ["response_type=code id_token", "resource=<app_id>"]
        ```

    5. Klicken Sie auf **Put**. 

Sobald Ihr Anbieter konfiguriert ist, können Sie im Tokenspeicher [das Aktualisierungstoken und die Ablaufzeit für das Zugriffstoken suchen](#retrieve-tokens-in-app-code). 

Um das Zugriffstoken jederzeit zu aktualisieren, rufen Sie einfach `/.auth/refresh` in einer beliebigen Sprache auf. Im folgenden Codeausschnitt wird jQuery verwendet, um Ihre Zugriffstoken aus einem JavaScript-Client zu aktualisieren.

```javascript
function refreshTokens() {
  let refreshUrl = "/.auth/refresh";
  $.ajax(refreshUrl) .done(function() {
    console.log("Token refresh completed successfully.");
  }) .fail(function() {
    console.log("Token refresh failed. See application logs for details.");
  });
}
```

Wenn ein Benutzer die Ihrer App gewährten Berechtigungen widerruft, schlägt Ihr Aufruf von `/.auth/me` möglicherweise mit der Antwort `403 Forbidden` fehl. Überprüfen Sie Ihre Anwendungsprotokolle auf Details, um Fehler zu diagnostizieren.

## <a name="extend-session-token-expiration-grace-period"></a>Verlängern der Toleranzperiode für das Sitzungstoken

Die authentifizierte Sitzung läuft nach acht Stunden ab. Nachdem eine authentifizierte Sitzung abgelaufen ist, gilt standardmäßig eine Toleranzperiode von 72 Stunden. Innerhalb dieser Toleranzperiode sind Sie berechtigt, das Sitzungstoken mit App Service zu aktualisieren, ohne den Benutzer erneut zu authentifizieren. Rufen Sie einfach `/.auth/refresh` auf, wenn Ihr Sitzungstoken ungültig wird. Sie müssen den Tokenablauf nicht selbst nachverfolgen. Nach Ablauf der 72-stündigen Toleranzperiode muss sich der Benutzer neu anmelden, um ein gültiges Sitzungstoken zu erhalten.

Wenn 72 Stunden für Sie nicht ausreichend sind, können Sie dieses Ablauffenster erweitern. Die Erweiterung des Ablauffensters über einen langen Zeitraum kann sich erheblich auf die Sicherheit auswirken (z.B. wenn ein Authentifizierungstoken kompromittiert oder gestohlen wird). Daher wird empfohlen, den Standardwert von 72 Stunden beizubehalten oder den Ablaufzeitraum auf den kleinsten Wert festzulegen.

Zum Erweitern des standardmäßigen Ablauffensters führen Sie den folgenden Befehl in [Cloud Shell](../cloud-shell/overview.md) aus.

```azurecli-interactive
az webapp auth update --resource-group <group_name> --name <app_name> --token-refresh-extension-hours <hours>
```

> [!NOTE]
> Die Toleranzperiode gilt nur für die authentifizierte App Service-Sitzung, nicht jedoch für die Token von den Identitätsanbietern. Es gibt keine Toleranzperiode für die abgelaufenen Anbietertoken. 
>

## <a name="limit-the-domain-of-sign-in-accounts"></a>Beschränken der Domäne von Anmeldekonten

Die Anmeldung über mehrere Domänen ist sowohl bei Microsoft-Konten als auch bei Azure Active Directory möglich. Microsoft-Konten lassen beispielsweise _outlook.com_-, _live.com_- und _hotmail.com_-Konten zu. Azure AD lässt eine beliebige Anzahl von benutzerdefinierten Domänen für die Anmeldekonten zu. Möglicherweise möchten Sie aber, dass Ihre Benutzer möglichst schnell direkt zu Ihrer eigenen Azure AD-Anmeldeseite (etwa `contoso.com`) gelangen. Gehen Sie folgendermaßen vor, um den Domänennamen der Anmeldekonten vorzuschlagen.

Navigieren Sie in [https://resources.azure.com](https://resources.azure.com) zu **subscriptions** > ** _\<subscription\_name_** > **resourceGroups** > **_ \<resource\_group\_name> _** > **providers** > **Microsoft.Web** > **sites** > **_ \<app\_name> _** > **config** > **authsettings**. 

Klicken Sie auf **Bearbeiten**, ändern Sie die folgende Eigenschaft, und klicken Sie dann auf **Put**. Achten Sie darauf, _\<domain\_name>_ durch die gewünschte Domäne zu ersetzen.

```json
"additionalLoginParams": ["domain_hint=<domain_name>"]
```

Diese Einstellung fügt den `domain_hint`-Abfragezeichenfolgen-Parameter an die Umleitungs-URL der Anmeldung an. 

> [!IMPORTANT]
> Der Client kann den `domain_hint`-Parameter nach dem Empfang der Umleitungs-URL entfernen und sich dann mit einer anderen Domäne anmelden. Diese Funktion ist zwar komfortabel, unter Sicherheitsaspekten aber bedenklich.
>

## <a name="authorize-or-deny-users"></a>Autorisieren oder Ablehnen von Benutzern

Während App Service den einfachsten Autorisierungsfall erledigt (also nicht authentifizierte Anforderungen ablehnt), erfordert Ihre App möglicherweise ein differenzierteres Autorisierungsverhalten, beispielsweise Einschränken des Zugriffs, sodass nur eine bestimmte Benutzergruppe Zugriff hat. Für bestimmte Fälle müssen Sie benutzerdefinierten Anwendungscode schreiben, um den Zugriff für einen angemeldeten Benutzer zuzulassen oder abzulehnen. In anderen Fällen kann App Service oder Ihr Identitätsanbieter möglicherweise unterstützen, ohne dass Codeänderungen erforderlich sind.

- [Serverebene](#server-level-windows-apps-only)
- [Identitätsanbieterebene](#identity-provider-level)
- [Anwendungsebene](#application-level)

### <a name="server-level-windows-apps-only"></a>Serverebene (nur Windows-Apps)

Für jede Windows-App können Sie das Autorisierungsverhalten des IIS-Webservers definieren, indem Sie die Datei *Web.config* bearbeiten. Linux-Apps verwenden IIS nicht und können nicht über *Web.config* konfiguriert werden.

1. Navigieren Sie zu `https://<app-name>.scm.azurewebsites.net/DebugConsole`.

1. Navigieren Sie im Browser-Explorer Ihrer App Service-Dateien zu *site/wwwroot*. Wenn keine *Web.config*-Datei vorhanden ist, erstellen Sie diese, indem Sie **+**  > **Neue Datei** auswählen. 

1. Wählen Sie das Freihandwerkzeug für *Web.config.* aus, um die Datei zu bearbeiten. Fügen Sie den folgenden Konfigurationscode hinzu, und klicken Sie auf **Speichern**. Wenn *Web.config* bereits vorhanden ist, fügen Sie einfach das `<authorization>`-Element mit allen zugehörigen Werten hinzu. Fügen Sie die Konten, die Sie zulassen möchten, im `<allow>`-Element hinzu.

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <configuration>
       <system.web>
          <authorization>
            <allow users="user1@contoso.com,user2@contoso.com"/>
            <deny users="*"/>
          </authorization>
       </system.web>
    </configuration>
    ```

### <a name="identity-provider-level"></a>Identitätsanbieterebene

Der Identitätsanbieter kann eine bestimmte fertige Autorisierung bereitstellen. Beispiel:

- Für [Azure App Service](configure-authentication-provider-aad.md) können Sie das [Verwalten von Zugriff auf Unternehmensebene](../active-directory/manage-apps/what-is-access-management.md) direkt in Azure AD vornehmen. Anweisungen hierzu finden Sie unter [Aufheben des Zugriffs eines Benutzers auf eine Anwendung](../active-directory/manage-apps/methods-for-removing-user-access.md).
- Für [Google](configure-authentication-provider-google.md) können Google-API-Projekte, die zu einer [Organisation](https://cloud.google.com/resource-manager/docs/cloud-platform-resource-hierarchy#organizations) gehören, so konfiguriert werden, dass sie Zugriff nur für Benutzer aus Ihrer Organisation zulassen (weitere Informationen finden Sie auf der [Unterstützungsseite **Setting up OAuth 2.0** von Google](https://support.google.com/cloud/answer/6158849?hl=en)).

### <a name="application-level"></a>Anwendungsschicht

Wenn keine der anderen Ebenen die erforderliche Autorisierung bereitstellt, oder wenn Ihre Plattform oder Ihr Identitätsanbieter nicht unterstützt wird, müssen Sie benutzerdefinierten Code schreiben, um Benutzer anhand der [Benutzeransprüche](#access-user-claims) zu autorisieren.

## <a name="updating-the-configuration-version"></a>Aktualisieren der Konfigurationsversion

Es gibt zwei Versionen der Verwaltungs-API für das Authentifizierungs-/Autorisierungsfeature. Die V2-Version ist für die „Authentifizierung“-Erfahrung im Azure-Portal erforderlich. Eine App, die bereits die V1-API verwendet, kann ein Upgrade auf die V2-Version vornehmen, sobald ein paar Änderungen vorgenommen wurden. Insbesondere muss die Geheimniskonfiguration in slotpersistente Anwendungseinstellungen verschoben werden. Dies kann automatisch über den Abschnitt „Authentifizierung“ im Portal für Ihre App erfolgen.

> [!WARNING]
> Die Migration zu V2 deaktiviert die Verwaltung des App Service-Authentifizierungs-/Autorisierungsfeatures für Ihre Anwendung durch einige Clients wie das Azure-Portal, die Azure CLI und Azure PowerShell. Dies lässt sich nicht rückgängig machen.

Die V2-API unterstützt kein Erstellen oder Bearbeiten eines Microsoft-Kontos als gesonderten Anbieter, wie es in V1 der Fall war. Stattdessen nutzt sie die konvergierte [Microsoft Identity Platform](../active-directory/develop/v2-overview.md), um Benutzer sowohl mit Azure AD als auch mit persönlichen Microsoft-Konten anzumelden. Wenn Sie zur V2-API wechseln, wird die V1-Konfiguration von Azure Active Directory verwendet, um den Microsoft Identity Platform-Anbieter zu konfigurieren. Der Microsoft-Kontoanbieter V1 wird in den Migrationsprozess übernommen und funktioniert weiterhin wie bisher. Es wird aber empfohlen, zum neueren Microsoft Identity Platform-Modell zu wechseln. Weitere Informationen finden Sie unter [Unterstützung für Microsoft-Kontoanbieterregistrierungen](#support-for-microsoft-account-provider-registrations).

Beim automatisierten Migrationsvorgang werden Anbietergeheimnisse in Anwendungseinstellungen verschoben, und der Rest der Konfiguration wird in das neue Format konvertiert. So verwenden Sie die automatische Migration

1. Navigieren Sie im Portal zu Ihrer App, und wählen Sie die Menüoption **Authentifizierung** aus.
1. Wenn die App mit dem V1-Modell konfiguriert wurde, wird eine Schaltfläche **Aktualisieren** angezeigt.
1. Überprüfen Sie die Beschreibung in der Bestätigungsaufforderung. Wenn Sie bereit sind, die Migration durchzuführen, klicken Sie in der Eingabeaufforderung auf **Upgrade**.

### <a name="manually-managing-the-migration"></a>Manuelles Verwalten der Migration

Mithilfe der folgenden Schritte können Sie die Anwendung manuell zur V2-API migrieren, wenn Sie nicht die oben genannte automatische Version verwenden möchten.

#### <a name="moving-secrets-to-application-settings"></a>Verschieben von Geheimnissen in Anwendungseinstellungen

1. Rufen Sie Ihre vorhandene Konfiguration ab, indem Sie die V1-API verwenden:

   ```azurecli
   # For Web Apps
   az webapp auth show -g <group_name> -n <site_name>
   ```

   Notieren Sie sich aus der resultierenden JSON-Nutzlast den Geheimniswert für jeden von Ihnen konfigurierten Anbieter:

   * AAD: `clientSecret`
   * Google: `googleClientSecret`
   * Facebook: `facebookAppSecret`
   * Twitter: `twitterConsumerSecret`
   * Microsoft-Konto: `microsoftAccountClientSecret`

   > [!IMPORTANT]
   > Die Geheimniswerte sind wichtige Sicherheitsanmeldeinformationen und sollten sorgfältig behandelt werden. Teilen Sie diese Werte nicht, und speichern Sie sie nicht dauerhaft auf einem lokalen Computer.

1. Erstellen Sie slotpersistente Anwendungseinstellungen für jeden Geheimniswert. Sie können den Namen jeder einzelnen Anwendungseinstellung auswählen. Ihr Wert sollte dem Wert entsprechen, den Sie im vorherigen Schritt abgerufen haben, oder [auf ein Key Vault-Geheimnis verweisen](./app-service-key-vault-references.md?toc=/azure/azure-functions/toc.json), das Sie mit diesem Wert erstellt haben.

   Um die Einstellung zu erstellen, können Sie das Azure-Portal verwenden oder für jeden Anbieter eine Variation der folgenden Aktionen ausführen:

   ```azurecli
   # For Web Apps, Google example    
   az webapp config appsettings set -g <group_name> -n <site_name> --slot-settings GOOGLE_PROVIDER_AUTHENTICATION_SECRET=<value_from_previous_step>

   # For Azure Functions, Twitter example
   az functionapp config appsettings set -g <group_name> -n <site_name> --slot-settings TWITTER_PROVIDER_AUTHENTICATION_SECRET=<value_from_previous_step>
   ```

   > [!NOTE]
   > Die Anwendungseinstellungen für diese Konfiguration müssen als slotpersistent gekennzeichnet werden, was bedeutet, dass Sie während eines [Slotaustauschvorgangs](./deploy-staging-slots.md) nicht zwischen Umgebungen verschoben werden. Der Grund hierfür ist, dass Ihre Authentifizierungskonfiguration selbst an die Umgebung gebunden ist. 

1. Erstellen Sie eine neue JSON-Datei namens `authsettings.json`. Nehmen Sie die Ausgabe, die Sie zuvor erhalten haben, und entfernen Sie daraus jeden Geheimniswert. Schreiben Sie die verbleibende Ausgabe in die Datei, und stellen Sie sicher, dass kein Geheimnis enthalten ist. In einigen Fällen kann die Konfiguration Arrays enthalten, die leere Zeichenfolgen enthalten. Stellen Sie sicher, dass dies auf `microsoftAccountOAuthScopes` nicht zutrifft, und wenn doch, ändern Sie diesen Wert in `null`.

1. Fügen Sie `authsettings.json` eine Eigenschaft hinzu, die auf den Namen der Anwendungseinstellung verweist, die Sie zuvor für jeden Anbieter erstellt haben:
 
   * AAD: `clientSecretSettingName`
   * Google: `googleClientSecretSettingName`
   * Facebook: `facebookAppSecretSettingName`
   * Twitter: `twitterConsumerSecretSettingName`
   * Microsoft-Konto: `microsoftAccountClientSecretSettingName`

   Nach diesem Vorgang könnte eine Beispieldatei etwa wie folgt aussehen, im vorliegenden Fall nur für AAD konfiguriert:

   ```json
   {
       "id": "/subscriptions/00d563f8-5b89-4c6a-bcec-c1b9f6d607e0/resourceGroups/myresourcegroup/providers/Microsoft.Web/sites/mywebapp/config/authsettings",
       "name": "authsettings",
       "type": "Microsoft.Web/sites/config",
       "location": "Central US",
       "properties": {
           "enabled": true,
           "runtimeVersion": "~1",
           "unauthenticatedClientAction": "AllowAnonymous",
           "tokenStoreEnabled": true,
           "allowedExternalRedirectUrls": null,
           "defaultProvider": "AzureActiveDirectory",
           "clientId": "3197c8ed-2470-480a-8fae-58c25558ac9b",
           "clientSecret": "",
           "clientSecretSettingName": "MICROSOFT_IDENTITY_AUTHENTICATION_SECRET",
           "clientSecretCertificateThumbprint": null,
           "issuer": "https://sts.windows.net/0b2ef922-672a-4707-9643-9a5726eec524/",
           "allowedAudiences": [
               "https://mywebapp.azurewebsites.net"
           ],
           "additionalLoginParams": null,
           "isAadAutoProvisioned": true,
           "aadClaimsAuthorization": null,
           "googleClientId": null,
           "googleClientSecret": null,
           "googleClientSecretSettingName": null,
           "googleOAuthScopes": null,
           "facebookAppId": null,
           "facebookAppSecret": null,
           "facebookAppSecretSettingName": null,
           "facebookOAuthScopes": null,
           "gitHubClientId": null,
           "gitHubClientSecret": null,
           "gitHubClientSecretSettingName": null,
           "gitHubOAuthScopes": null,
           "twitterConsumerKey": null,
           "twitterConsumerSecret": null,
           "twitterConsumerSecretSettingName": null,
           "microsoftAccountClientId": null,
           "microsoftAccountClientSecret": null,
           "microsoftAccountClientSecretSettingName": null,
           "microsoftAccountOAuthScopes": null,
           "isAuthFromFile": "false"
       }   
   }
   ```

1. Übermitteln Sie diese Datei als neue Authentifizierungs-/Autorisierungskonfiguration für Ihre App:

   ```azurecli
   az rest --method PUT --url "/subscriptions/<subscription_id>/resourceGroups/<group_name>/providers/Microsoft.Web/sites/<site_name>/config/authsettings?api-version=2020-06-01" --body @./authsettings.json
   ```

1. Überprüfen Sie, ob Ihre App nach dieser Geste weiterhin erwartungsgemäß funktioniert.

1. Löschen Sie die in den vorherigen Schritten verwendete Datei.

Sie haben die App nun migriert, um Geheimnisse von Identitätsanbietern als Anwendungseinstellungen zu speichern.

#### <a name="support-for-microsoft-account-provider-registrations"></a>Unterstützung für Microsoft-Kontoanbieterregistrierungen

Wenn Ihre vorhandene Konfiguration einen Microsoft-Kontoanbieter, aber keinen Azure Active Directory-Anbieter enthält, können Sie die Konfiguration auf den Azure Active Directory-Anbieter umstellen und dann die Migration durchführen. Gehen Sie dazu folgendermaßen vor:

1. Wechseln Sie zu [**App-Registrierungen**](https://portal.azure.com/#blade/Microsoft_AAD_RegisteredApps/ApplicationsListBlade) im Azure-Portal, und suchen Sie die Registrierung, die Ihrem Microsoft-Kontoanbieter zugeordnet ist. Möglicherweise befindet sie sich unter der Überschrift „Anwendungen aus persönlichem Konto“.
1. Navigieren Sie zur Seite „Authentifizierung“ für die Registrierung. Unter „Umleitungs-URIs“ sollte ein Eintrag angezeigt werden, der auf `/.auth/login/microsoftaccount/callback`endet. Kopieren Sie diesen URI.
1. Fügen Sie einen neuen URI hinzu, der mit dem soeben kopierten übereinstimmt, außer dass er auf `/.auth/login/aad/callback` endet. Dies ermöglicht, dass die Registrierung von der Konfiguration der App Service-Authentifizierung/-Autorisierung verwendet werden kann.
1. Navigieren Sie zu der App Service-Authentifizierungs-/Autorisierungskonfiguration für Ihre App.
1. Sammeln Sie die Konfiguration für den Microsoft-Kontoanbieter.
1. Konfigurieren Sie den Azure Active Directory-Anbieter mithilfe des Verwaltungsmodus „Erweitert“, wobei Sie die Client-ID und die geheimen Clientschlüsselwerte bereitstellen, die Sie im vorherigen Schritt erfasst haben. Verwenden Sie für die Aussteller-URL `<authentication-endpoint>/<tenant-id>/v2.0`, und ersetzen Sie *\<authentication-endpoint>* durch den [Authentifizierungsendpunkt für Ihre Cloudumgebung](../active-directory/develop/authentication-national-cloud.md#azure-ad-authentication-endpoints) (z. B. „https://login.microsoftonline.com“ für globales Azure), und ersetzen Sie außerdem *\<tenant-id>* durch Ihre **Verzeichnis (Mandanten)-ID**.
1. Nachdem Sie die Konfiguration gespeichert haben, testen Sie den Anmeldeablauf, indem Sie in Ihrem Browser zum Endpunkt `/.auth/login/aad` in Ihrem Standort navigieren und den Anmeldevorgang durchführen.
1. An diesem Punkt haben Sie die Konfiguration erfolgreich übernommen, aber die vorhandene Konfiguration des Microsoft-Kontoanbieters bleibt erhalten. Bevor Sie sie entfernen, stellen Sie sicher, dass alle Teile Ihrer App über Anmeldelinks auf den Azure Active Directory-Anbieter verweisen usw. Vergewissern Sie sich, dass alle Teile Ihrer App erwartungsgemäß funktionieren.
1. Nachdem Sie überprüft haben, ob der AAD Azure Active Directory-Anbieter funktioniert, können Sie die Konfiguration des Microsoft-Kontoanbieters entfernen.

> [!WARNING]
> Es ist möglich, die beiden Registrierungen durch Ändern der [unterstützten Kontotypen](../active-directory/develop/supported-accounts-validation.md) für die AAD-App-Registrierung zu konvergieren. Dies würde jedoch eine neue Zustimmungsaufforderung für Benutzer von Microsoft-Konten erzwingen, und die Identitätsansprüche dieser Benutzer können sich in der Struktur unterscheiden, weil `sub` Werte signifikant ändert, da eine neue App-ID verwendet wird. Dieser Ansatz wird nur empfohlen, wenn Sie sich über die Konsequenzen vollkommen bewusst sind. Warten Sie stattdessen lieber, bis Unterstützung für die zwei Registrierungen in der V2-API-Oberfläche bereitgestellt wird.

#### <a name="switching-to-v2"></a>Wechseln zu V2

Nachdem Sie die obigen Schritte ausgeführt haben, navigieren Sie im Azure-Portal zu der App. Wählen Sie den Abschnitt „Authentifizierung (Vorschau)“ aus. 

Alternativ können Sie eine PUT-Anforderung an die `config/authsettingsv2`-Ressource unterhalb der Standortressource stellen. Das Schema für die Nutzlast ist das selbe wie das im Abschnitt [Konfigurieren mithilfe einer Datei](#config-file) erfasste.

## <a name="configure-using-a-file-preview"></a><a name="config-file"> </a>Konfigurieren mithilfe einer Datei (Vorschau)

Ihre Authentifizierungseinstellungen können optional über eine Datei konfiguriert werden, die in Ihrer Bereitstellung enthalten ist. Dies ist möglicherweise für bestimmte Vorschaufunktionen der Authentifizierung bzw. Autorisierung in App Service erforderlich.

> [!IMPORTANT]
> Denken Sie daran, dass die Nutzlast Ihrer App – und damit auch diese Datei – möglicherweise zwischen Umgebungen verschoben wird, z. B. bei [Slots](./deploy-staging-slots.md). Es ist sehr wahrscheinlich, dass an jeden Slot eine andere App-Registrierung angeheftet werden muss. In diesen Fällen sollten Sie weiterhin die Standardkonfigurationsmethode anstelle der Konfigurationsdatei verwenden.

### <a name="enabling-file-based-configuration"></a>Aktivieren der dateibasierten Konfiguration

> [!CAUTION]
> Während der Vorschau deaktiviert die Aktivierung der dateibasierten Konfiguration die Verwaltung des App Service-Authentifizierungs-/Autorisierungsfeatures für Ihre Anwendung durch einige Clients wie das Azure-Portal, die Azure CLI und Azure PowerShell.

1. Erstellen Sie im Stammverzeichnis Ihres Projekts (bereitgestellt in D:\home\site\wwwroot in Ihrer Web-App bzw. Funktions-App) eine neue JSON-Datei für Ihre Konfiguration. Geben Sie die gewünschten Konfigurationsdetails entsprechend der [Referenz zur dateibasierten Konfiguration](#configuration-file-reference) an. Wenn Sie eine vorhandene Azure Resource Manager-Konfiguration ändern, stellen Sie sicher, dass Sie die in der `authsettings`-Sammlung aufgezeichneten Eigenschaften in die Konfigurationsdatei übersetzen.

2. Ändern Sie die vorhandene Konfiguration, die in den [Azure Resource Manager](../azure-resource-manager/management/overview.md)-APIs unter `Microsoft.Web/sites/<siteName>/config/authsettings` aufgezeichnet ist. Hierfür können Sie eine [Azure Resource Manager-Vorlage](../azure-resource-manager/templates/overview.md) oder ein Tool wie den [Azure-Ressourcen-Explorer](https://resources.azure.com/) verwenden. In der authsettings-Sammlung müssen Sie drei Eigenschaften festlegen (und möglicherweise andere Eigenschaften entfernen):

    1.  Legen Sie `enabled` auf TRUE fest.
    2.  Legen Sie `isAuthFromFile` auf TRUE fest.
    3.  Legen Sie `authFilePath` auf den Namen der Zertifikatdatei fest (z. B. „auth.json“).

> [!NOTE]
> Das Format für `authFilePath` variiert je nach Plattform. Unter Windows werden sowohl relative als auch absolute Pfade unterstützt. Relative Pfade werden empfohlen. Unter Linux werden derzeit nur absolute Pfade unterstützt. Daher sollte der Wert der Einstellung „/home/site/wwwroot/auth.json“ oder ähnlich lauten.

Nachdem Sie dieses Konfigurationsupdate vorgenommen haben, wird der Inhalt der Datei verwendet, um das Verhalten der App Service-Authentifizierung bzw. -Autorisierung für diesen Standort zu definieren. Wenn Sie zur Azure Resource Manager-Konfiguration zurückkehren möchten, legen Sie `isAuthFromFile` wieder auf FALSE fest.

### <a name="configuration-file-reference"></a>Verweis von der Konfigurationsdatei

Alle Geheimnisse, auf die von Ihrer Konfigurationsdatei verwiesen wird, müssen als [Anwendungseinstellungen](./configure-common.md#configure-app-settings) gespeichert werden. Sie können die Einstellungen ganz nach Wunsch benennen. Stellen Sie lediglich sicher, dass die Verweise von der Konfigurationsdatei dieselben Schlüssel verwenden.

Im Folgenden finden Sie mögliche Konfigurationsoptionen in der Datei:

```json
{
    "platform": {
        "enabled": <true|false>
    },
    "globalValidation": {
        "unauthenticatedClientAction": "RedirectToLoginPage|AllowAnonymous|Return401|Return403",
        "redirectToProvider": "<default provider alias>",
        "excludedPaths": [
            "/path1",
            "/path2"
        ]
    },
    "httpSettings": {
        "requireHttps": <true|false>,
        "routes": {
            "apiPrefix": "<api prefix>"
        },
        "forwardProxy": {
            "convention": "NoProxy|Standard|Custom",
            "customHostHeaderName": "<host header value>",
            "customProtoHeaderName": "<proto header value>"
        }
    },
    "login": {
        "routes": {
            "logoutEndpoint": "<logout endpoint>"
        },
        "tokenStore": {
            "enabled": <true|false>,
            "tokenRefreshExtensionHours": "<double>",
            "fileSystem": {
                "directory": "<directory to store the tokens in if using a file system token store (default)>"
            },
            "azureBlobStorage": {
                "sasUrlSettingName": "<app setting name containing the sas url for the Azure Blob Storage if opting to use that for a token store>"
            }
        },
        "preserveUrlFragmentsForLogins": <true|false>,
        "allowedExternalRedirectUrls": [
            "https://uri1.azurewebsites.net/",
            "https://uri2.azurewebsites.net/",
            "url_scheme_of_your_app://easyauth.callback"
        ],
        "cookieExpiration": {
            "convention": "FixedTime|IdentityDerived",
            "timeToExpiration": "<timespan>"
        },
        "nonce": {
            "validateNonce": <true|false>,
            "nonceExpirationInterval": "<timespan>"
        }
    },
    "identityProviders": {
        "azureActiveDirectory": {
            "enabled": <true|false>,
            "registration": {
                "openIdIssuer": "<issuer url>",
                "clientId": "<app id>",
                "clientSecretSettingName": "APP_SETTING_CONTAINING_AAD_SECRET",
            },
            "login": {
                "loginParameters": [
                    "paramName1=value1",
                    "paramName2=value2"
                ]
            },
            "validation": {
                "allowedAudiences": [
                    "audience1",
                    "audience2"
                ]
            }
        },
        "facebook": {
            "enabled": <true|false>,
            "registration": {
                "appId": "<app id>",
                "appSecretSettingName": "APP_SETTING_CONTAINING_FACEBOOK_SECRET"
            },
            "graphApiVersion": "v3.3",
            "login": {
                "scopes": [
                    "public_profile",
                    "email"
                ]
            },
        },
        "gitHub": {
            "enabled": <true|false>,
            "registration": {
                "clientId": "<client id>",
                "clientSecretSettingName": "APP_SETTING_CONTAINING_GITHUB_SECRET"
            },
            "login": {
                "scopes": [
                    "profile",
                    "email"
                ]
            }
        },
        "google": {
            "enabled": true,
            "registration": {
                "clientId": "<client id>",
                "clientSecretSettingName": "APP_SETTING_CONTAINING_GOOGLE_SECRET"
            },
            "login": {
                "scopes": [
                    "profile",
                    "email"
                ]
            },
            "validation": {
                "allowedAudiences": [
                    "audience1",
                    "audience2"
                ]
            }
        },
        "twitter": {
            "enabled": <true|false>,
            "registration": {
                "consumerKey": "<consumer key>",
                "consumerSecretSettingName": "APP_SETTING_CONTAINING TWITTER_CONSUMER_SECRET"
            }
        },
        "apple": {
            "enabled": <true|false>,
            "registration": {
                "clientId": "<client id>",
                "clientSecretSettingName": "APP_SETTING_CONTAINING_APPLE_SECRET"
            },
            "login": {
                "scopes": [
                    "profile",
                    "email"
                ]
            }
        },
        "openIdConnectProviders": {
            "<providerName>": {
                "enabled": <true|false>,
                "registration": {
                    "clientId": "<client id>",
                    "clientCredential": {
                        "clientSecretSettingName": "<name of app setting containing client secret>"
                    },
                    "openIdConnectConfiguration": {
                        "authorizationEndpoint": "<url specifying authorization endpoint>",
                        "tokenEndpoint": "<url specifying token endpoint>",
                        "issuer": "<url specifying issuer>",
                        "certificationUri": "<url specifying jwks endpoint>",
                        "wellKnownOpenIdConfiguration": "<url specifying .well-known/open-id-configuration endpoint - if this property is set, the other properties of this object are ignored, and authorizationEndpoint, tokenEndpoint, issuer, and certificationUri are set to the corresponding values listed at this endpoint>"
                    }
                },
                "login": {
                    "nameClaimType": "<name of claim containing name>",
                    "scopes": [
                        "openid",
                        "profile",
                        "email"
                    ],
                    "loginParameterNames": [
                        "paramName1=value1",
                        "paramName2=value2"
                    ],
                }
            },
            //...
        }
    }
}
```

## <a name="pin-your-app-to-a-specific-authentication-runtime-version"></a>Anheften Ihrer App an eine bestimmte Authentifizierungsruntimeversion

Wenn Sie die Authentifizierung/Autorisierung aktivieren, wird die Plattformmiddleware in Ihre HTTP-Anforderungspipeline eingefügt, wie in der [Featureübersicht](overview-authentication-authorization.md#how-it-works) beschrieben. Diese Plattformmiddleware wird im Rahmen der routinemäßigen Plattformupdates in regelmäßigen Abständen mit neuen Features und Verbesserungen aktualisiert. Standardmäßig wird Ihre Web- oder Funktions-App auf der neuesten Version dieser Plattformmiddleware ausgeführt. Diese automatischen Updates sind immer abwärtskompatibel. In dem seltenen Fall, dass dieses automatische Update ein Runtimeproblem bei Ihrer Web- oder Funktions-App verursacht, können Sie jedoch vorübergehend ein Rollback zur vorherigen Middlewareversion ausführen. In diesem Artikel wird erläutert, wie Sie eine App temporär einer bestimmten Version der Authentifizierungsmiddleware anheften.

### <a name="automatic-and-manual-version-updates"></a>Automatische und manuelle Versionsupdates 

Sie können Ihre App einer bestimmten Version der Plattformmiddleware anheften, indem Sie eine `runtimeVersion`-Einstellung für die App vornehmen. Ihre App wird immer auf der neuesten Version ausgeführt, es sei denn, Sie möchten sie explizit einer bestimmten Version anheften. Es werden mehrere Versionen gleichzeitig unterstützt. Wenn Sie sie einer ungültigen Version anheften, die nicht mehr unterstützt wird, verwendet Ihre App stattdessen die neueste Version. Um immer die neueste Version auszuführen, legen Sie `runtimeVersion` auf ~1 fest. 

### <a name="view-and-update-the-current-runtime-version"></a>Anzeigen und Aktualisieren der aktuellen Runtimeversion

Sie können die von der App verwendete Runtimeversion ändern. Die neue Runtimeversion sollte nach dem Neustart der App wirksam werden. 

#### <a name="view-the-current-runtime-version"></a>Anzeigen der aktuellen Runtimeversion

Sie können die aktuelle Version der Plattformauthentifizierungs-Middleware entweder mithilfe der Azure CLI oder über einen der HTTP-Endpunkte der integrierten Version in ihrer App anzeigen.

##### <a name="from-the-azure-cli"></a>Über die Azure CLI

Zeigen Sie mithilfe der Azure CLI die aktuelle Middlewareversion mit dem Befehl [az webapp auth show](/cli/azure/webapp/auth#az_webapp_auth_show) an.

```azurecli-interactive
az webapp auth show --name <my_app_name> \
--resource-group <my_resource_group>
```

Ersetzen Sie in diesem Code `<my_app_name>` durch den Namen der App. Ersetzen Sie außerdem `<my_resource_group>` durch den Namen der Ressourcengruppe für Ihre App.

Das Feld `runtimeVersion` wird in der CLI-Ausgabe angezeigt. Es ähnelt der folgenden Beispielausgabe, die zur besseren Lesbarkeit beschnitten wurde: 
```output
{
  "additionalLoginParams": null,
  "allowedAudiences": null,
    ...
  "runtimeVersion": "1.3.2",
    ...
}
```

##### <a name="from-the-version-endpoint"></a>Vom Versionsendpunkt

Sie können auch über den /.auth/version-Endpunkt einer App die aktuelle Middlewareversion anzeigen, auf der die App ausgeführt wird. Es ähnelt der folgenden Beispielausgabe:
```output
{
"version": "1.3.2"
}
```

#### <a name="update-the-current-runtime-version"></a>Aktualisieren der aktuellen Runtimeversion

Mithilfe der Azure CLI können Sie die `runtimeVersion`-Einstellung in der App mit dem Befehl [az webapp auth update](/cli/azure/webapp/auth#az_webapp_auth_update) aktualisieren.

```azurecli-interactive
az webapp auth update --name <my_app_name> \
--resource-group <my_resource_group> \
--runtime-version <version>
```

Ersetzen Sie `<my_app_name>` durch den Namen Ihrer App. Ersetzen Sie außerdem `<my_resource_group>` durch den Namen der Ressourcengruppe für Ihre App. Ersetzen Sie außerdem `<version>` durch eine gültige Version der Runtime 1.x oder durch `~1` für die aktuelle Version. Die Anmerkungen zu den verschiedenen Runtimeversionen finden Sie [hier] (https://github.com/Azure/app-service-announcements), um die Version zu ermitteln, der die App angeheftet werden soll.

Sie können diesen Befehl an der [Azure Cloud Shell](../cloud-shell/overview.md) ausführen, indem Sie im vorangehenden Codebeispiel **Ausprobieren** auswählen. Sie können auch die [Azure-Befehlszeilenschnittstelle lokal](/cli/azure/install-azure-cli) zum Ausführen dieses Befehls verwenden, nachdem Sie sich mit [az login](/cli/azure/reference-index#az_login) angemeldet haben.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Tutorial: Umfassendes Authentifizieren und Autorisieren von Benutzern in Azure App Service](tutorial-auth-aad.md)
