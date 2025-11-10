# blocage-enfants.ps1
# Ce script bloque les principaux sites inadaptés pour les enfants
# via le fichier hosts de Windows (redirection vers 127.0.0.1)

$hostsPath = "$env:SystemRoot\System32\drivers\etc\hosts"

$sites = @(
    # --- Réseaux sociaux ---
    "youtube.com","www.youtube.com","m.youtube.com","youtu.be",
    "facebook.com","www.facebook.com","m.facebook.com","instagram.com","www.instagram.com",
    "tiktok.com","www.tiktok.com","vm.tiktok.com","snapchat.com","www.snapchat.com",
    "twitter.com","x.com","www.twitter.com","www.x.com","reddit.com","www.reddit.com",
    "discord.com","www.discord.com","pinterest.com","www.pinterest.com","twitch.tv","www.twitch.tv",

    # --- Sites pour adultes ---
    "pornhub.com","xvideos.com","xnxx.com","redtube.com","youporn.com","adultfriendfinder.com",

    # --- Jeux d’argent ---
    "bet365.com","pokerstars.com","unibet.com","pmu.fr","winamax.fr","parionssport.fdj.fr",

    # --- Streaming / contenus potentiellement addictifs ---
    "netflix.com","www.netflix.com","primevideo.com","www.primevideo.com",
    "hbo.com","www.hbo.com","crunchyroll.com","www.crunchyroll.com"
)

Write-Host "🔒 Ajout des sites bloqués dans le fichier hosts..."

foreach ($site in $sites) {
    $entry = "127.0.0.1 $site"
    # Vérifie si déjà présent
    if (-not (Select-String -Path $hostsPath -Pattern $site -Quiet)) {
        Add-Content -Path $hostsPath -Value $entry
        Write-Host "→ Bloqué : $site"
    } else {
        Write-Host "✓ Déjà bloqué : $site"
    }
}

Write-Host ""
Write-Host "✅ Blocage terminé !"
Write-Host "➡️ Redémarre ton navigateur ou ton PC pour appliquer les changements."
