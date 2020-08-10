---
title: "Refine on Click"
date: 2020-08-10T15:30:51+02:00
draft: false
---


# Utiliser une carte comme un sélecteur et afficher les résultats sur le coté

Ce template propose une vue dynamique sur 2 niveaux. D'abord on propose une carte dont les éléments sont clicables.
- Les informations générales sont affichées sur le coté.
- Ensuite, quand l'utilisateur clique sur le carte, cela ouvre un premier niveau avec des informations relatives à la sélection.
- Pour finir, si l'utilisateur veut en savoir plus sur cette sélection, un second tiroir s'ouvre pour prendre la quasi-totalité de l'écran.

Ce template est la meilleure alternative lorsque vous souhaitez afficher une quantité importante d'information et que cela ne peut tout simplement pas rentrer dans l'infobulle par défaut !
N.B.: la zone sombre aide l'utilisateur à revenir sur la carte rapidement pour changer de sélection

{{< highlight js >}}
<div class="ods-box donotcopy-specific"
     ng-init="donotcopy = { 'simulaterefineonclick' : false}">
    <div class="map-drawer-container"
         ng-class="{
                   'map-drawer-container--active': donotcopy.simulaterefineonclick,
                   'map-drawer-container--active-full': drawer.status}"
         ng-init="drawer = {'status':false}">    
        <div class="map-drawer-container__map">
            <div class="map-drawer-container__backdrop"
                 ng-click="donotcopy.simulaterefineonclick = undefined;
                           drawer.status = false"
                 ng-class="{'map-drawer-container__backdrop--active': donotcopy.simulaterefineonclick }">
            </div>
            <h1>
                Map
            </h1>
            <div class="ods-button"
                 ng-click="donotcopy.simulaterefineonclick = true">Refine-on-click</div>
        </div>
        <div class="map-drawer-container__info">
            <h2>
                General info here
            </h2>
        </div>
        <div class="map-drawer-container__drawer map-drawer-container__drawer__partial">                      
            <div class="map-drawer-container__drawer__close"
                 ng-click="donotcopy.simulaterefineonclick = undefined;
                           drawer.status = false">
                <i class="fa fa-times"></i>
            </div>
            <h2>
                Specific info here
            </h2>
            <div class="ods-button"
                 ng-click="drawer.status = !drawer.status">
                Expend / See more
            </div>
        </div>
        <div class="map-drawer-container__drawer map-drawer-container__drawer__full">
            <h2>
                More info here
            </h2>
        </div>
    </div>
</div>
{{ / highlight }}
