# --- EXTRAIT : Panneau de Contrôle des Salons Vocaux (UI Views) ---
class VCView(discord.ui.View):
    def __init__(self, owner_id, vc_id):
        super().__init__(timeout=None)
        self.owner_id, self.vc_id = owner_id, vc_id
        self.locked, self.hidden = False, False

    # Sécurité : Vérifie que seul le propriétaire du salon peut utiliser les boutons
    async def interaction_check(self, interaction: discord.Interaction) -> bool:
        if interaction.user.id != self.owner_id:
            await interaction.response.send_message("⛔ Vous n'êtes pas le propriétaire.", ephemeral=True)
            return False
        return True

    @discord.ui.button(label="Verrouiller", style=discord.ButtonStyle.success, emoji="🔒", row=1)
    async def lock(self, interaction: discord.Interaction, button: discord.ui.Button):
        vc = interaction.guild.get_channel(self.vc_id)
        self.locked = not self.locked
        
        # Modification des permissions du salon en temps réel
        await vc.set_permissions(interaction.guild.default_role, connect=not self.locked)
        
        # Mise à jour dynamique de l'interface (bouton rouge/vert)
        button.style = discord.ButtonStyle.danger if self.locked else discord.ButtonStyle.success
        button.label = "Déverrouiller" if self.locked else "Verrouiller"
        await interaction.response.edit_message(view=self)
