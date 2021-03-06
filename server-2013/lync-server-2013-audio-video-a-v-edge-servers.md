﻿---
title: Audio/Video (A/V) Edge Server in Lync Server 2013
TOCTitle: Audio/Video (A/V) Edge Server in Lync Server 2013
ms:assetid: b0cc538b-77eb-47fb-be82-5ab0631c6219
ms:mtpsurl: https://technet.microsoft.com/it-it/library/JJ721852(v=OCS.15)
ms:contentKeyID: 49887711
ms.date: 08/24/2015
mtps_version: v=OCS.15
ms.translationtype: HT
---

# Audio/Video (A/V) Edge Server in Lync Server 2013

 

_**Ultima modifica dell'argomento:** 2012-11-01_

Il servizio A/V Edge consente agli utenti interni, ovvero agli utenti connessi alla rete dell'organizzazione, di condividere audio e video con utenti esterni, che non sono connessi alla rete dell'organizzazione. Oltre ad audio e video, il servizio A/V Edge inoltre offre il supporto per attività come la condivisione del desktop e il trasferimento di file.

Il servizio A/V Edge viene principalmente gestito mediante la configurazione A/V Edge. Queste impostazioni consentono di gestire la quantità massima di larghezza di banda da allocare per porta e per utente e di specificare per quanto tempo può essere utilizzato un token di autenticazione prima che debba essere rinnovato. Le impostazioni di configurazione A/V Edge possono essere applicate ai siti o ai singoli A/V Edge Server. Quando si determina la raccolta di impostazioni che avrà la priorità, utilizzare come riferimento le informazioni seguenti:

  - Le impostazioni configurate nell'ambito del servizio, ovvero in un singolo server, hanno la priorità su tutto.

  - Le impostazioni configurate nell'ambito del sito hanno la priorità su quelle configurate nell'ambito globale. Le impostazioni nell'ambito del servizio avranno tuttavia la precedenza sulle impostazioni nell'ambito del sito.

  - Le impostazioni nell'ambito globale verranno utilizzate solo se non vi sono impostazioni di servizio configurate nel singolo server e se non vi sono impostazioni per il sito in cui si trova tale server.

Il servizio A/V Edge può essere gestito solo utilizzando Lync Server PowerShell e i cmdlet CsAVEdgeConfiguration.

## Argomenti della sezione

  - [Restituire informazioni sulla configurazione di A/V Edge Server](lync-server-2013-return-a-v-edge-server-configuration-information.md)

  - [Creare o modificare una raccolta di impostazioni di configurazione di A/V Edge Server](lync-server-2013-create-or-modify-a-collection-of-a-v-edge-server-configuration-settings.md)

  - [Eliminare una raccolta esistente di impostazioni di configurazione di A/V Edge Server.](lync-server-2013-delete-an-existing-collection-of-a-v-edge-server-configuration-settings.md)

