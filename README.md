# blocage-enfants.ps1
# Ce script bloque les principaux sites inadaptés pour les enfants
# via le fichier hosts de Windows (redirection vers 127.0.0.1)

# Vérification des droits administrateur
if (-not ([Security.Principal.WindowsPrincipal] [Security.Principal.WindowsIdentity]::GetCurrent()).IsInRole(`
    [Security.Principal.WindowsBuiltInRole] "Administrator")) {
    Write-Host "❌ Ce script doit être exécuté en tant qu'administrateur." -ForegroundColor Red
    Write-Host "➡️ Clique droit sur le fichier et choisis 'Exécuter en tant qu'administrateur'."
    exit
}

$hostsPath = "$env:SystemRoot\System32\drivers\etc\hosts"

if (-not (Test-Path $hostsPath)) {
    Write-Host "❌ Fichier hosts introuvable à l'emplacement : $hostsPath" -ForegroundColor Red
    exit
}

$sites = @(
    # --- Réseaux sociaux ---
    "youtube.com","www.youtube.com","m.youtube.com","youtu.be",
    "facebook.com","www.facebook.com","m.facebook.com",
    "instagram.com","www.instagram.com",
    "tiktok.com","www.tiktok.com","vm.tiktok.com",
    "snapchat.com","www.snapchat.com",
    "twitter.com","x.com","www.twitter.com","www.x.com",
    "reddit.com","www.reddit.com",
    "discord.com","www.discord.com",
    "pinterest.com","www.pinterest.com",
    "twitch.tv","www.twitch.tv",

    # --- Sites pour adultes ---
    "pornhub.com","xvideos.com","xnxx.com","redtube.com","youporn.com","adultfriendfinder.com",

    # --- Jeux d’argent ---
    "bet365.com","pokerstars.com","unibet.com","pmu.fr","winamax.fr","parionssport.fdj.fr",

    # --- Streaming / contenus potentiellement addictifs ---
    "netflix.com","www.netflix.com",
    "primevideo.com","www.primevideo.com",
    "hbo.com","www.hbo.com",
    "crunchyroll.com","www.crunchyroll.com"
)

Write-Host ""
Write-Host "🔒 Ajout des sites bloqués dans le fichier hosts..." -ForegroundColor Cyan

foreach ($site in $sites) {
    $entry = "127.0.0.1 $site"
    if (-not (Select-String -Path $hostsPath -Pattern "^\s*127\.0\.0\.1\s+$site(\s|$)" -Quiet)) {
        Add-Content -Path $hostsPath -Value $entry -Encoding UTF8
        Write-Host "→ Bloqué : $site" -ForegroundColor Yellow
    } else {
        Write-Host "✓ Déjà bloqué : $site" -ForegroundColor Green
    }
}

Write-Host ""
Write-Host "✅ Blocage terminé !" -ForegroundColor Green
Write-Host "➡️ Redémarre ton navigateur ou ton PC pour appliquer les changements."

