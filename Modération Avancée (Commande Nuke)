# --- EXTRAIT : Fonction de réinitialisation de salon (Nuke) ---
@bot.tree.command(name="nuke", description="☢️ Réinitialiser le salon.")
@app_commands.checks.has_permissions(manage_channels=True)
async def nuke(interaction: discord.Interaction):
    # 1. Sauvegarde de la position actuelle dans la hiérarchie du serveur
    pos = interaction.channel.position
    
    # 2. Clonage du salon (conserve les permissions et la catégorie)
    new_channel = await interaction.channel.clone()
    
    # 3. Suppression de l'ancien salon (efface tout l'historique)
    await interaction.channel.delete()
    
    # 4. Replacement du nouveau salon à la position exacte de l'ancien
    await new_channel.edit(position=pos)
