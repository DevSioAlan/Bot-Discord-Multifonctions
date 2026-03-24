# 🛠️ Bot Discord Multi-Fonctions - Modération, Économie & Vocaux Dynamiques

> **Note :** Ce dépôt met en avant la logique de création d'un bot Discord polyvalent développé par Alan. Il illustre la maîtrise de l'interface utilisateur moderne de Discord (UI Views, Modals), la gestion d'une base de données relationnelle locale (SQLite3) et l'implémentation de commandes d'administration avancées.

## 🚀 À propos du projet
Ce bot a été pensé comme le couteau suisse ultime pour un serveur Discord. Plutôt que de se limiter à des commandes textuelles, il exploite pleinement les nouveautés de l'API Discord en offrant des panneaux de contrôle interactifs sous forme de boutons et menus déroulants. Il inclut également un mini-jeu de stratégie intégrant une économie virtuelle avec sa propre logique mathématique.

---

## ✨ Fonctionnalités Clés

* 🎙️ **Générateur de Salons Vocaux Dynamiques :** Création de salons éphémères avec un panneau de contrôle UI privé (Verrouillage, expulsion, changement de nom via des formulaires "Modals").
* 🛡️ **Modération Avancée & Anti-Raid :** Filtre anti-insultes automatique, commande `/nuke` pour nettoyer un salon corrompu, système de `warn` persistant.
* 💰 **Économie & Mini-Jeux :** Système bancaire (`/work`, `/pay`, `/slots`) et un jeu exclusif de conquête ("World at Arms") basé sur un algorithme de force pondéré.
* 💾 **Base de Données Locale (SQLite3) :** Sauvegarde sécurisée de l'économie, des niveaux d'XP et des avertissements.

---

## 💻 Code Highlights (Extraits Choisis)

### 1. UI/UX : Panneau de Contrôle Vocal Interactif (Discord Views)
Utilisation avancée de `discord.ui.View` et `discord.ui.Button` pour créer une interface utilisateur cliquable, remplaçant les anciennes commandes textuelles fastidieuses.

```python
class VCView(discord.ui.View):
    def __init__(self, owner_id, vc_id):
        super().__init__(timeout=None)
        self.owner_id, self.vc_id = owner_id, vc_id
        self.locked = False

    # Sécurisation des interactions : seul le créateur du salon peut utiliser les boutons
    async def interaction_check(self, interaction: discord.Interaction) -> bool:
        if interaction.user.id != self.owner_id:
            await interaction.response.send_message("⛔ Vous n'êtes pas le propriétaire.", ephemeral=True)
            return False
        return True

    @discord.ui.button(label="Verrouiller", style=discord.ButtonStyle.success, emoji="🔒", row=1)
    async def lock(self, interaction: discord.Interaction, button: discord.ui.Button):
        vc = interaction.guild.get_channel(self.vc_id)
        self.locked = not self.locked
        
        # Modification dynamique des permissions du salon
        await vc.set_permissions(interaction.guild.default_role, connect=not self.locked)
        
        # Mise à jour visuelle du bouton en temps réel (Vert -> Rouge)
        button.style = discord.ButtonStyle.danger if self.locked else discord.ButtonStyle.success
        button.label = "Déverrouiller" if self.locked else "Verrouiller"
        await interaction.response.edit_message(view=self)
