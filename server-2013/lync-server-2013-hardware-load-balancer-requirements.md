﻿---
title: 'Lync Server 2013: Requisiti relativi al servizio di bilanciamento del carico hardware'
TOCTitle: Requisiti relativi al servizio di bilanciamento del carico hardware
ms:assetid: 32891268-2059-43d0-adf4-af4ff1e9ce66
ms:mtpsurl: https://technet.microsoft.com/it-it/library/JJ656815(v=OCS.15)
ms:contentKeyID: 49887512
ms.date: 08/24/2015
mtps_version: v=OCS.15
ms.translationtype: HT
---

# Requisiti relativi al servizio di bilanciamento del carico hardware per Lync Server 2013

 

_**Ultima modifica dell'argomento:** 2015-05-11_

La topologia perimetrale consolidata di Lync Server 2013 con scalabilità implementata è ottimizzata per il bilanciamento del carico DNS per le nuove distribuzioni che attuano la federazione principalmente con organizzazioni in cui viene usato Lync Server. Se è richiesta la disponibilità elevata per uno degli scenari seguenti, sarà necessario usare un servizio di bilanciamento del carico hardware nei pool di server perimetrale nei casi seguenti:

  - Federazione con organizzazioni che usano Office Communications Server 2007 R2 o Office Communications Server 2007

  - Messaggistica unificata di Exchange per utenti remoti che usano la Messaggistica unificata di Exchange precedente a Exchange 2010 con SP1

  - Connettività con utenti di messaggistica istantanea pubblica

> [!IMPORTANT]  
> Non è possibile usare il bilanciamento del carico DNS in un'interfaccia e il bilanciamento del carico hardware in un'altra. È necessario usare lo stesso tipo di bilanciamento del carico per entrambe le interfacce, in quanto non è supportata una combinazione dei due tipi.


> [!NOTE]
> Se si usa un servizio di bilanciamento del carico hardware, il bilanciamento del carico distribuito per le connessioni con la rete interna deve essere configurato per bilanciare il carico solo del traffico verso i server che eseguono i servizi Access Edge e A/V Edge. Non è possibile bilanciare il carico del traffico verso il servizio Web Conferencing Edge interno o il servizio del proxy XMPP interno.




> [!NOTE]
> La modalità DSR (Direct Server Return) NAT non è supportata con Lync Server 2013.



Per determinare se il bilanciamento del carico hardware supporta le funzionalità necessarie richieste da Lync Server 2013, vedere "Partner produttori di servizi di bilanciamento del carico di Lync Server 2010" all'indirizzo [http://go.microsoft.com/fwlink/p/?linkId=202452](http://go.microsoft.com/fwlink/p/?linkid=202452).

## Requisiti dei servizi di bilanciamento del carico hardware per i server perimetrali che eseguono il servizio A/V Edge

Vengono riportati di seguito i requisiti dei servizi di bilanciamento del carico hardware per i server perimetrali che eseguono il servizio A/V Edge:

  - Disattivare il nagling TCP per le porte 443 interne ed esterne. Per nagling si intende il processo di combinazione di più pacchetti di piccole dimensioni in un singolo pacchetto di dimensioni maggiori per una trasmissione più efficiente.

  - Disattivare il nagling TCP per l'intervallo di porte esterne compreso tra 50.000 e 59.999.

  - Non usare NAT nel firewall interno o esterno.

  - L'interfaccia interna perimetrale deve trovarsi in una rete diversa rispetto all'interfaccia esterna del server perimetrale e il routing tra di esse deve essere disabilitato.

  - L'interfaccia esterna del server perimetrale che esegue il servizio A/V Edge deve usare indirizzi IP instradabili pubblicamente e non la conversione NAT o delle porte in uno degli indirizzi IP esterni.

## Requisiti relativi al servizio di bilanciamento del carico hardware

I requisiti di affinità basati sui cookie sono stati notevolmente ridotti in Lync Server 2013 per i servizi Web. Se si distribuisce Lync Server 2013 e non si intende conservare i Front End Server o pool Front End di Lync Server 2010, non è necessaria la persistenza basata sui cookie. Se, tuttavia, si intende conservare temporaneamente o permanentemente i Front End Server o i pool Front End di Lync Server 2010, è necessario continuare a usare la persistenza basata sui cookie distribuita e configurata per Lync Server 2010.


> [!NOTE]
> <STRONG>La decisione di usare l'affinità basata sui cookie anche se la distribuzione non la richiede</STRONG> non determina un impatto negativo.



Per le distribuzioni in cui l'affinità basata sui cookie **non verrà usata**:

  - Nella regola di pubblicazione del proxy inverso per la porta 4443, impostare **Forward host header** su True in modo da assicurarsi che l'URL originale venga inoltrato.

Per le distribuzioni in cui l'affinità basata sui cookie **verrà usata**:

  - Nella regola di pubblicazione del proxy inverso per la porta 4443, impostare **Forward host header** su True in modo da assicurarsi che l'URL originale venga inoltrato.

  - Il cookie del bilanciamento del carico hardware NON DEVE essere contrassegnato come httpOnly

  - Per il bilanciamento del carico hardware NON DEVE essere impostata una data di scadenza

  - Il cookie del bilanciamento del carico hardware DEVE essere denominato **MS-WSMAN** ovvero il valore, non modificabile, previsto dai servizi Web

  - Il cookie del bilanciamento del carico hardware DEVE essere impostato in ogni risposta HTTP per la quale la richiesta HTTP in arrivo non presentava un cookie, indipendentemente dal fatto che una precedente risposta HTTP nella stessa connessione TCP abbia già ottenuto un cookie. Se il bilanciamento del carico ottimizza l'inserimento del cookie in modo che venga eseguito una sola volta per ogni connessione TCP, tale ottimizzazione NON DEVE essere usata


> [!NOTE]
> Le tipiche configurazioni del bilanciamento del carico hardware usano l'affinità degli indirizzi di origine e una durata della sessione TCP di 20 minuti, ovvero un valore ottimale per i client Lync Server e Lync 2013 in quanto lo stato della sessione viene mantenuto durante l'uso del client e/o l'interazione con un'applicazione.



Se si distribuiscono dispositivi mobili, il servizio di bilanciamento del carico hardware deve essere in grado di bilanciare il carico delle singole richieste in una sessione TCP (in effetti, è necessario essere in grado di bilanciare il carico di una singola richiesta in base all'indirizzo IP di destinazione).


> [!WARNING]
> I servizi di bilanciamento del carico hardware F5 presentano una funzionalità chiamata OneConnect che garantisce che il bilanciamento del carico venga eseguito per ogni singola richiesta all'interno di una connessione TCP. Se si distribuiscono dispositivi mobili, verificare che il fornitore di servizi di bilanciamento del carico hardware supporti la stessa funzionalità. Le applicazioni per dispositivi mobili Apple iOS più recenti richiedono TLS (Transport Layer Security) versione 1.2. F5 offre impostazioni specifiche a questo scopo.<BR>Per informazioni dettagliate sui servizi di bilanciamento del carico hardware di terze parti, vedere <A href="http://go.microsoft.com/fwlink/p/?linkid=230700">http://go.microsoft.com/fwlink/p/?linkId=230700</A>



Vengono riportati di seguito i requisiti dei servizi di bilanciamento del carico hardware per i servizi Web dei pool di server Director e Front End:

  - Per gli IP virtuali dei servizi Web interni, impostare la persistenza Source\_addr (porta interna 80, 443) nel servizio di bilanciamento del carico hardware. Per Lync Server 2013, la persistenza Source\_addr prevede l'invio di più connessioni provenienti da un singolo indirizzo IP a un server per mantenere lo stato della sessione.

  - Impostare il timeout di inattività TCP su 1800 secondi.

  - Nel firewall tra il proxy inverso e il servizio di bilanciamento del carico hardware del pool di hop successivi creare una regola per consentire il traffico HTTPS nella porta 4443, dal proxy inverso al servizio di bilanciamento del carico hardware. Il servizio di bilanciamento del carico hardware deve essere configurato per l'ascolto sulle porte 80, 443 e 4443.

## Riepilogo dei requisiti per l'affinità del servizio di bilanciamento del carico hardware


<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th>Posizione client/utente</th>
<th>Requisiti per l'affinità FQDN dei servizi Web esterni</th>
<th>Requisiti per l'affinità FQDN dei servizi Web interni</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Lync Web App (utenti interni ed esterni)</p>
<p>Dispositivo mobile (utenti interni ed esterni)</p></td>
<td><p>Nessuna affinità</p></td>
<td><p>Affinità indirizzo di origine</p></td>
</tr>
<tr class="even">
<td><p>Lync Web App (solo utenti esterni)</p>
<p>Dispositivo mobile (utenti interni ed esterni)</p></td>
<td><p>Nessuna affinità</p></td>
<td><p>Affinità indirizzo di origine</p></td>
</tr>
<tr class="odd">
<td><p>Lync Web App (solo utenti interni)</p>
<p>Dispositivo mobile (non distribuito)</p></td>
<td><p>Nessuna affinità</p></td>
<td><p>Affinità indirizzo di origine</p></td>
</tr>
</tbody>
</table>


## Monitoraggio delle porte per i servizi di bilanciamento del carico hardware

Il monitoraggio delle porte viene definito nei servizi di bilanciamento del carico hardware per stabilire quando determinati servizi non sono più disponibili a causa di un errore hardware o delle comunicazioni. Se, ad esempio, il servizio Front End Server (RTCSRV) si interrompe a causa di un errore del Front End Server o del pool Front End, anche il monitoraggio del servizio di bilanciamento del carico hardware (HLB) dovrebbe interrompere la ricezione del traffico nei servizi Web. Il monitoraggio delle porte viene implementato nel servizio HLB per monitorare quanto segue:

### Pool di utenti di Front End Server - Interfaccia interna HLB

<table>
<colgroup>
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
</colgroup>
<thead>
<tr class="header">
<th>IP virtuale/Porta</th>
<th>Porta nodo</th>
<th>Computer/Monitor nodo</th>
<th>Profilo persistenza</th>
<th>Note</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>&lt;pool&gt;web-int_mco_443_vs</p>
<p>443</p></td>
<td><p>443</p></td>
<td><p>Front End</p>
<p>5061</p></td>
<td><p>Origine</p></td>
<td><p>HTTPS</p></td>
</tr>
<tr class="even">
<td><p>&lt;pool&gt;web-int_mco_80_vs</p>
<p>80</p></td>
<td><p>80</p></td>
<td><p>Front End</p>
<p>5061</p></td>
<td><p>Origine</p></td>
<td><p>HTTP</p></td>
</tr>
</tbody>
</table>


### Pool di utenti di Front End Server - Interfaccia esterna HLB

<table>
<colgroup>
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
</colgroup>
<thead>
<tr class="header">
<th>IP virtuale/Porta</th>
<th>Porta nodo</th>
<th>Computer/Monitor nodo</th>
<th>Profilo persistenza</th>
<th>Note</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>&lt;pool&gt;web_mco_443_vs</p>
<p>443</p></td>
<td><p>4443</p></td>
<td><p>Front End</p>
<p>5061</p></td>
<td><p>Nessuno</p></td>
<td><p>HTTPS</p></td>
</tr>
<tr class="even">
<td><p>&lt;pool&gt;web_mco_80_vs</p>
<p>80</p></td>
<td><p>8080</p></td>
<td><p>Front End</p>
<p>5061</p></td>
<td><p>Nessuno</p></td>
<td><p>HTTP</p></td>
</tr>
</tbody>
</table>

