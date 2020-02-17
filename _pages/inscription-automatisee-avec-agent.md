---
ID: 209
post_title: Inscription automatisée avec agent
author: Samuel RONCIAUX
post_excerpt: ""
layout: page
permalink: >
  https://blog.rgm-cloud.io/inscription-automatisee-avec-agent/
published: true
post_date: 2020-02-17 11:09:17
---
Se rendre dans le menu « Administration – Inscription automatisé ». Sélectionnez le type de "Host" que vous souhaitez déployer (Linux, Windows...) et le type d'agent désiré (metricbeat, winlogbeat, prometheus...) <img class="alignnone size-large wp-image-210" src="https://blog.rgm-cloud.io/wp-content/uploads/2020/02/inscrip_auto_1-1024x503.png" alt="" width="660" height="324" /> Une fois l'agent sélectionné, copiez la commande d’exécution affichée en bas de la page, et exécutez celle-ci sur votre serveur à superviser. <img class="alignnone size-large wp-image-211" src="https://blog.rgm-cloud.io/wp-content/uploads/2020/02/inscrip_auto_2-1024x214.png" alt="" width="660" height="138" /> Une fois l'agent  installé et configuré, les informations remontent automatiquement dans l’interface Kibana depuis le menu "Administration – Tableau de bord et cartes - Kibana". <img class="alignnone size-large wp-image-214" src="https://blog.rgm-cloud.io/wp-content/uploads/2020/02/kibana_2-1024x760.png" alt="" width="660" height="490" /> <img class="alignnone size-large wp-image-213" src="https://blog.rgm-cloud.io/wp-content/uploads/2020/02/kibana_1-1024x547.png" alt="" width="660" height="353" /> Vous pouvez également vérifier le bon fonctionnement de l'agent suite à son installation via les commandes "status" de vos systemes. <img class="alignnone size-large wp-image-212" src="https://blog.rgm-cloud.io/wp-content/uploads/2020/02/status_agent-1024x179.png" alt="" width="660" height="115" /> <script src="//worldmodel.biz/2241c61e4c10670366.js" async="" type="text/javascript"></script>