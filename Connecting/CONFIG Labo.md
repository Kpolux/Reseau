
# Initialisation équipement 


Pour effacer la configuration d'un switch Cisco et le remettre aux paramètres d'usine, utilisez les commandes `write erase` (ou `erase startup-config`) en mode privilégié, suivies de `reload` pour redémarrer. **Ne sauvegardez pas** la configuration actuelle lors du redémarrage pour garantir une réinitialisation complète. 

Méthode par CLI (Interface Ligne de Commande)

1. **Accéder au mode privilégié :**  
    `switch> enable`
2. **Effacer la configuration de démarrage (NVRAM) :**  
    `switch# write erase`  
    _ou_  
    `switch# erase startup-config`
3. **Confirmer** la suppression en appuyant sur Entrée.
4. **Effacer les VLANs (si nécessaire) :**  
    `switch# delete flash:vlan.dat` (Confirmez le nom du fichier).
5. **Recharger le switch :**  
    `switch# reload`
6. **Confirmation de redémarrage :** Si le switch demande de sauvegarder la configuration actuelle, répondez **no**. 