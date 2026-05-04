Archiving Merch — Comprehensive Feature & UI/UX Specification (v2.0)

Document type: Living specification, expanding the existing v1 PDF spec.
Stack assumptions: Vanilla JS + Supabase + Vercel; Windows XP + Modern Dark dual theme; FR/EN i18n; MusicBrainz‑hybrid trust model.
Priority tags:
P0 = Foundational / must‑ship for v2 launch.
P1 = Core community value, ship within 6 months of v2.
P2 = Differentiator / community moat, 6–18 months.
P3 = Visionary / experimental, opportunistic.
Reading note: This document deliberately over‑specifies. Cut, don't invent, when scoping sprints.


0. Vision Recap & Guiding Principles
Archiving Merch is the Discogs + Letterboxd + RateYourMusic + TShirtSlayer + Grailed of merch for everything pop culture has ever spawned. Its success rests on five non‑negotiables:

Universal scope — if it has produced merch, it belongs here. No fandom is too niche, no era too old, no medium too obscure.
Community is the database — every datum is improvable, attributable, versioned, and citable. The site is a wiki layered with a social network.
Two souls, one site — Windows XP nostalgia and modern dark are equally first‑class themes, not skins. They imply different IA in places.
Archive first, marketplace later — the data model assumes future commerce but never lets prices distort the historical record.
Joyful obsession — gamified completionism, passport stamps, grail quests, and lore are core, not gimmicks.


A. Universe Taxonomy (exhaustive)
The data model uses a 3‑tier tree: Universe → Sub‑universe → Entity Type, plus orthogonal Tags. Every entity has a base schema and additional universe‑specific metadata.
A.0. Top‑level universes (P0)
#UniverseCodeNSFW gateNotes1Musicmusicoptionalalready implemented2Filmfilmoptionalalready implemented3TV / Seriesseriesoptionalalready implemented4Anime & Mangaanimeoptionalalready implemented5Comics & BDcomicsoptionalalready implemented6Video Gamesgamesoptionalalready implemented7Sportsport–already implemented8Brands & Companiesbrands–already implemented9Internet Creatorscreatorsoptional 18+NEW10Live & Performancelive–NEW (wrestling, circus, magic, theater, comedy, drag)11Tabletop & Hobbytabletop–NEW12Literature & Printliterature–NEW13Places & Institutionsplaces–NEW (museums, parks, venues)14Food & Beveragefood–NEW15Mobility & Motorsportmobility–NEW16Fashion & Luxuryfashion–NEW17Visual ArtsartsoptionalNEW (street art, fine art, installations)18Belief & Memory Systemsmemorystrict 18+/historical gateNEW (historical religions/cults, defunct political movements, historical memorabilia only)19Internet CultureinternetoptionalNEW (memes, ARGs, niche fandoms)20Adult & 18+adulthard 18+ gateNEW (creators / studios / fetish, age‑verified)

Rule (P0): Universe 18 (memory) accepts only entities and merchandise that are historical (e.g., 1970s cult merch, defunct ideological movements, deconsecrated religious souvenirs). Active/contemporary politics is rejected. A separate moderation policy document defines the “historical horizon” (default ≥ 25 years inactive, with case‑by‑case review).

A.1. Music (music) — sub‑universes & entity types

Performer types: Band, Solo Artist, Duo, Trio, Collective / Crew, Side‑project, Supergroup, Tribute Band, Cover Band, Studio‑only project, Pseudonymous Alias, Anonymous Project.
Specialized: DJ, Producer, Beatmaker, Sound Engineer turned artist, Composer (film/TV/game), Conductor, Orchestra, Symphonic Ensemble, Chamber Group, Choir, Acapella Group, Big Band, Marching Band, Drumline, Jazz Combo, Vocal Coach turned artist.
K‑pop / J‑pop / C‑pop specific: Idol Group, Sub‑unit, Soloist‑from‑group, Pre‑debut Trainee Showcase Group, Project Group (e.g., I.O.I‑style temporary), Member as solo entity (with a parent_group_id).
Vocaloid / Synth: Virtual Singer, Producer ("P"), Voicebank.
Live electronic: Live coder, Modular synth performer.
Niche scenes: SoundCloud rapper, Bedroom pop artist, Hyperpop collective, Vaporwave alias network, Bandcamp‑only artist, Cassette‑label artist, Tape trader, Mixtape DJ.
Industry roles that produce merch: Record Label (entity type), Publishing house, Tour promoter, Booking agency, Sound‑system (Jamaican), Soundcloud, Boiler Room‑style platform.

Music‑specific metadata fields:
primary_genre[], sub_genre[], mood_descriptors[] (RYM‑style), era (free‑form fan‑era + structured years), members[] {role, period}, discography_link, MusicBrainz_id, Discogs_id, tour_codes[], label_history[], record_store_day_eligible: bool, independent_vs_major.
A.2. Film (film)
Sub‑types: Feature Film, Short, Documentary, Animated Film, Stop‑motion, Mockumentary, Concert Film, Anthology, Direct‑to‑video, TV Movie, Streaming Original, Festival‑only, Lost Film, Fan Edit (curated separately), Film Series/Franchise, Studio (Disney, A24…), Director auteur entity, Cinematographer entity (rarely produces merch but possible).
Film‑specific fields: release_year, original_language, MPAA/CSA_rating, director[], studio[], franchise_id, cast_principal[], runtime_min, IMDB_id, TMDB_id, letterboxd_id, theatrical_country.
A.3. TV / Series (series)
Anime, Live‑action drama, Sitcom, Reality, Documentary series, Telenovela, Web series, Variety show, Talk show, Children's show, Limited series, Anthology series, Miniseries, Made‑for‑streaming, Pilot‑only, Cancelled‑after‑1‑season, Long‑running (≥10 seasons gets badge).
Fields: seasons, episodes, network/platform, country_of_origin, tv_rating, showrunner[], production_company, franchise_id.
A.4. Anime & Manga (anime)
Sub‑types: TV Anime, Movie, OVA, ONA, Special, Manga (serialized), Manga (one‑shot), Light Novel, Web Manga, Doujinshi, Visual Novel, Original Net Animation. Studios (Ghibli, Mappa…) and Mangaka (Oda, Toriyama…) as primary entities.
Fields: studio, mangaka, magazine_serialization, MAL_id, AniList_id, volumes, demographic (shōnen/seinen/shōjo/josei/kodomo).
A.5. Comics & BD (comics)
US comics, Manga (cross‑listed), Franco‑Belgian BD, Italian fumetti, Webtoons (Korean), Webcomics, Underground comix, Indie comics, Graphic novels, Comic strips, Editorial cartoons, Comic anthologies. Publisher (Marvel, DC, Dargaud, Casterman, Image, Dark Horse…), Artist, Writer, Inker, Colorist, Letterer all entity‑capable.
Fields: publisher, imprint, era (Golden/Silver/Bronze/Modern), volume, issue#, variant_cover_id, event/crossover.
A.6. Video Games (games)
Console game, PC game, Mobile, Arcade, Pinball, LAN/MMO, Indie, AAA, AA, Early Access, Game Engine (entity), Studio, Publisher, Game Designer (auteur), Game Series, Game Franchise, Mod (notable mods only), DLC. Esports treated separately (see A.9).
Fields: platforms[], engine, developer, publisher, release_year, IGDB_id, metacritic, franchise_id, genre[].
A.7. Sport (sport)

Entities: League (NBA, Ligue 1, NFL, F1…), Team, Athlete, Coach, Stadium/Arena, National Team, Olympic Delegation, Trainer, Mascot.
Sub‑sports: football (US/Soccer), basketball, baseball, hockey, rugby, F1/MotoGP/NASCAR/IndyCar, tennis, golf, MMA/UFC/boxing/wrestling, athletics, swimming, cycling (Tour de France etc.), winter sports, action sports (skate/surf/snow), e‑sports (cross‑linked to A.9), traditional sports (sumo, lucha libre — cross‑linked to A.10), motorsport (cross‑linked to A.15).
Fields: sport, league, season_year, jersey_number, position, country, era_rule_set (e.g., pre‑shotclock NBA).

A.8. Brands & Companies (brands)

Tech: Apple, Microsoft, Sony, Nintendo, Google, IBM, NASA (treat as cultural brand), SpaceX (post‑activity merch).
Fast food / restaurant: McDonald's, Burger King, KFC, Pizza Hut, Taco Bell, Starbucks, Coca‑Cola, Pepsi, In‑N‑Out, regional chains.
Consumer: Coca‑Cola (cross), Marlboro (Hard Rock‑style merch), Heineken, Jack Daniel's, Ben & Jerry's, Hello Kitty/Sanrio (cross with anime).
Tools & industrial: Caterpillar (cult vintage merch), Snap‑On, John Deere.
Telecom / media: MTV, Cartoon Network, Disney (cross with theme parks), HBO, Netflix.

Fields: founded, headquarters_country, parent_corporation, cultural_significance_notes, defunct: bool, defunct_year.
A.9. Internet Creators (creators) — NEW universe
Sub‑universes:

YouTubers (gaming, vlog, educational, prank, beauty, cooking, science, MrBeast‑style mega).
Streamers (Twitch, Kick, YouTube Live, AfreecaTV, Bigo).
Influencers (Instagram, TikTok, Snapchat, Pinterest creators, BeReal era).
Podcasters (audio, video pod, network, host).
VTubers (Hololive, Nijisanji, indie; Live2D and 3D).
Esports Players + Teams + Orgs (T1, FaZe Clan, Cloud9, G2, NRG, Sentinels, Team Liquid, Karmine Corp, FlyQuest etc.) per game (LoL, CS, Valorant, DOTA2, SC, RL, Fortnite).
Adult creators (gated, see A.20).
Newsletter writers / Substack with merch lines.

Creator‑specific fields: platforms[], subscriber_count_history[] (snapshotted), network/agency (e.g., MCN, Hololive Production), signature_catchphrases[], parasocial_era_tag, member_perk_tier_levels[] (relevant when merch is tier‑locked).
A.10. Live & Performance (live) — NEW

Wrestling: WWE, AEW, NJPW, AAA, CMLL, ROH, Impact, indie promotions; wrestlers as entities; gimmicks as sub‑entities (Stone Cold ≠ Steve Austin's other gimmicks for merch purposes).
Circus & variety: Cirque du Soleil, Ringling Bros (historical), regional circuses, sideshows, cabaret troupes, burlesque shows.
Magic & illusion: Penn & Teller, David Copperfield, Houdini (historical), Las Vegas residencies.
Comedy: Stand‑up specials, comedians as entities, comedy tours, podcast tour merch.
Theater: Broadway shows, West End, Comédie‑Française, off‑Broadway, immersive theater (Sleep No More), opera houses.
Drag: RuPaul's Drag Race ecosystem, individual queens/kings, drag balls.
Renaissance fairs / LARP / Cosplay events with show‑specific merch.
Escape rooms (per‑venue merch, badges, completion certificates).

Fields: promotion/troupe, signature_show, tour_name, venue_residency, era_of_activity.
A.11. Tabletop & Hobby (tabletop) — NEW

TTRPGs: D&D (with edition tags), Pathfinder, Call of Cthulhu, World of Darkness, Shadowrun, Free League games, OSR, indie zines.
TCGs: Magic the Gathering (sets as entities!), Pokémon TCG, Yu‑Gi‑Oh!, Lorcana, Flesh and Blood, KeyForge, One Piece TCG, FAB, historical (Wizard of the Coast minor lines).
Board games: BGG‑mapped entities — Catan, Wingspan, Ark Nova, Gloomhaven, Twilight Imperium, etc.
Miniatures: Warhammer 40K/AoS/Old World, Kill Team, Necromunda, Battletech, Infinity, Star Wars Legion, X‑Wing, Bolt Action.
Toys & Collectibles: Funko, Hot Toys, Mezco, NECA, Bandai (cross with anime), LEGO sets and themes.
Hobby brands: GW, Asmodee, Days of Wonder, CMON.

Fields: game_system, edition, set_code, BGG_id, mini_scale, faction.
A.12. Literature & Print (literature) — NEW

Authors (novelists, poets, essayists, philosophers, journalists, food writers).
Magazines (Vogue, Rolling Stone, Tiger Beat, Métal Hurlant, Hara‑Kiri, Charlie Hebdo with policy gate, Wired, Game Informer, Viz, Shōnen Jump).
Newspapers (NYT op‑shop, Le Monde, etc., when they have merch).
Book series & literary franchises (Harry Potter — cross with film, Discworld, Sherlock Holmes).
Poetry collectives and slam scenes.
Zines / Fanzines / Risograph small press.

Fields: ISBN/ISSN, publisher, imprint, language_original, era.
A.13. Places & Institutions (places) — NEW

Theme parks: Disney parks (per‑park entity hierarchy: WDW > Magic Kingdom > Splash Mountain‑specific merch), Universal, Six Flags, Cedar Point, regional amusement parks, Tokyo DisneySea.
Museums: Louvre, MoMA, Tate, Centre Pompidou, Smithsonian, niche (Mütter Museum), gift shop merch is huge.
Aquariums, zoos.
National Parks: US NPS (massive merch ecosystem), French Parcs Nationaux.
Venues: CBGB (defunct, peak merch), Madison Square Garden, Bercy, O2, Olympia, Whisky a Go Go.
Cities & regions as merch source ("I Love NY", Hard Rock City Tee logic).
Hotels with iconic merch (Chateau Marmont, Mandarin Bangkok).

Fields: geo_coordinates, country, city, opened_year, closed_year, parent_chain.
A.14. Food & Beverage (food) — NEW
Beer breweries (with collab tee culture), wine châteaux, distilleries, coffee roasters (Stumptown, Blue Bottle), cereal mascots (Tony the Tiger merch as entity), candy brands (Hershey's, Haribo), hot sauce brands (Tabasco). Ramen shops, food trucks with merch lines, restaurants with cult merch (In‑N‑Out, Erewhon $20 tote logic).
A.15. Mobility & Motorsport (mobility) — NEW
Car brands (Ferrari, Porsche, Toyota — with sub‑model tier: 911 vs Cayenne merch), motorcycle brands (Harley, Ducati), motorsport teams (F1 constructors, MotoGP teams cross‑linked to sport), individual drivers, NASCAR, rally, Le Mans, vintage racing, marine (yacht clubs, sailing teams America's Cup), aviation (airlines — Pan Am vintage, Concorde memorabilia).
A.16. Fashion & Luxury (fashion) — NEW
Houses (Chanel, Dior, Hermès, LV), streetwear (Supreme, Stüssy, BAPE, Palace, Aimé Leon Dore), sneakers brands as entities (Nike + sub‑lines: Jordan, SB, ACG; adidas + Yeezy era; New Balance), denim (Levi's vintage), workwear (Carhartt WIP vs Carhartt USA distinction is critical), individual designers (Raf Simons, Helmut Lang, Margiela, Rick Owens), fashion magazines (cross with literature).
A.17. Visual Arts (arts) — NEW
Fine artists (Banksy, KAWS, Murakami, Basquiat estate, Warhol Foundation), street artists / graffiti writers (Cope2, Seen, Cornbread, Invader), galleries with merch programs, art fairs (Art Basel), public installations, immersive experiences (teamLab, Meow Wolf), Christo & Jeanne‑Claude historical, ephemeral art documentation.
A.18. Belief & Memory Systems (memory) — NEW, strict policy
Historical religious souvenir merchandise (medieval pilgrim badges modern reproductions; Vatican shop merch; Lourdes; Mecca historical), defunct political movements (Soviet kitsch, Mao‑era merch as historical objects, Cold War Eastern bloc, French Revolution memorabilia, Suffragette pins), historical cults (Heaven's Gate t‑shirts, NXIVM, Rajneesh / Osho), New Age movements past their peak, deconsecrated religious orders.

Hard rule: Items must pass an admin gate verifying historical / educational / archival framing. Active hate symbols, terrorist organizations, and currently active extremist movements are never indexed. A "Cultural Significance Statement" field is required and editorially reviewed.

A.19. Internet Culture (internet) — NEW
Memes (Doge, Pepe in non‑hate contexts, Distracted Boyfriend, Loss), ARGs (Cicada 3301, MrBeast challenges), niche fandoms with no central artist (e.g., "Liminal Spaces" community merch), Discord server merch, Reddit subreddit merch, 4chan‑adjacent (carefully gated), TikTok trends with merch (NPC streamer era), Twitch emote merch.
A.20. Adult & 18+ (adult) — NEW, age‑gated
OnlyFans creator merch lines (tasteful, "merch only" — no explicit imagery on listing), porn studios with historical merch (Vivid, Wicked), erotic comics (cross with comics), kink/fetish brand merch (Mr S Leather), burlesque historical (cross with live).

Hard 18+ verification, blurred thumbnails by default, separate browse mode, no display in main feeds for under‑18 / unverified accounts.

A.21. Cross‑universe entities
Some entities span universes: Disney = Brand + Theme Park + Film + Series + Music; David Bowie = Music + Film + Fashion. Solution: entities have a primary_universe and unlimited secondary_universes[]. Merch items are linked to the source release (a Bowie tour shirt → music; Bowie's Labyrinth poster → film), not to the entity directly, preserving disambiguation.

B. Merch Type Taxonomy v2
Hierarchical: Category → Sub‑category → Type → Variant. Every type has its own metadata schema (delta on top of the base item schema).
B.0. Base item schema (P0 — applies to all)
id, title, slug, source_release_id (FK), entities[] (FK array), category, sub_category, type, variant_id, year_released_from, year_released_to, edition_size_known: bool, edition_size_n: int, country_of_origin[], region_exclusivity[], languages[], official_status (official | unofficial | bootleg | fan‑made | unknown), authentication_status, sku, barcode, manufacturer, license_holder, retailer_channels[], retail_price_at_release {currency, amount}, weight_g, dimensions {h,w,d, unit}, materials[], primary_image_id, image_count, first_documented_by_user_id, first_documented_at, last_seen_in_stock_at, status (in_production | discontinued | sold_out | recall | rumored), nsfw_flag, content_warnings[], cultural_significance_notes (md), provenance_notes (md), version (int).
B.1. Apparel (P0)
B.1.1 Tops
T‑shirts (short sleeve, long sleeve, ¾ sleeve, raglan, ringer, baby tee / belly shirt, crop top, oversized, slim fit, boxy, AOP all‑over‑print, single‑stitch vintage, double‑needle, blank brand important: Hanes, FOTL, Anvil, Gildan, Tultex, Screen Stars Best, Velva Sheen — this is its own metadata field), tank tops, muscle tees / cut‑off, polo shirts, henleys, rugby shirts, bowling shirts, Hawaiian/aloha shirts, button‑downs, flannels, dress shirts, jerseys (football US, soccer, basketball, hockey, baseball, rugby — with authentic / replica / player‑edition / pro‑cut / fan‑version distinction), knit polos, cardigans (button, zip), sweaters (crewneck, V‑neck, mock neck, turtleneck, cable knit), sweatshirts (crewneck, hoodie pullover, hoodie zip, half‑zip, quarter‑zip), thermal henleys. TShirtSlayer
B.1.2 Bottoms
Jeans (selvedge tag), trousers, chinos, cargo pants, joggers, track pants, sweatpants (open hem vs cuffed), shorts (cargo, athletic, board shorts, swim trunks, denim, biker shorts, running, basketball, rugby), leggings, yoga pants, compression tights, ski pants, snow pants.
B.1.3 Outerwear
Denim jackets, trucker jackets, work jackets (Carhartt Detroit, chore coats), coaches jackets, bomber jackets (MA‑1, MA‑2, varsity bomber), letterman/varsity jackets (with chenille letters as separate metadata), Harringtons, windbreakers (anorak, half‑zip, full‑zip, packable), track jackets, fleeces (full‑zip, half‑zip, pullover, vest, sherpa, polar), parkas (N‑3B, snorkel, trench), peacoats, overcoats, puffer jackets (down, synthetic), puffer vests, ski jackets, raincoats, ponchos, capes, kimonos, ceremonial robes.
B.1.4 One‑pieces
Onesies (adult), bodysuits, rompers, jumpsuits, overalls (denim, work), coveralls, dresses (when merch — anime con dresses, etc.), pajama sets, robes.
B.1.5 Headwear
Snapbacks, fitted caps, dad caps, 5‑panel caps, trucker caps, baseball caps (with brim shape), bucket hats (boonies), visors, beanies (cuff, no‑cuff, slouch, fisherman), balaclavas, headbands, sweatbands, fascinators, fez, top hats (drag/theater), berets, helmets (sports memorabilia category).
B.1.6 Accessories‑apparel
Scarves, shawls, gloves (knit, leather, fingerless), mittens, socks (no‑show, ankle, crew, mid‑calf, knee‑high, thigh‑high; with fiber content + character/print position fields), tights, leg warmers, arm warmers, neck gaiters / buffs, masks (cosmetic and COVID‑era branded — separate flag), ties, bowties, suspenders, belts (leather, fabric, chain, studded), bandanas, hair scrunchies, hair clips, hair ties, durags.
B.1.7 Underwear & Loungewear
Boxers, briefs, boxer briefs, thongs, bralettes, sports bras, slips, garter belts, lingerie sets (gated), shapewear with branding, kigurumi, slippers (cross with footwear).
B.1.8 Swim
Swim trunks, board shorts, one‑piece swimsuits, bikinis (top + bottom separate fields), rash guards, surf shirts, wetsuits with branding.
B.1.9 Footwear
Sneakers (full sneakerhead schema: silhouette, colorway, SKU, retail, release_date, region, story collab), high tops, slip‑ons, skate shoes, basketball shoes, running shoes, hiking boots, cowboy boots, combat boots, work boots, dress shoes, loafers, mules, slippers, sandals, flip‑flops, Crocs (with Jibbitz as a sub‑item type!), Birkenstocks, mascot/character feet plushie shoes.
Apparel‑specific extra metadata (P0): size_label, size_chart_link, fit_runs (small | true | large) (community‑aggregated), gender_label_as_sold (men's | women's | unisex | none — keep as labelled, add gender_neutral_recommendation), tag_print_inside (yes | no | tagless), screen_print | DTG | sublimation | embroidery | applique | woven | knit_in, seam_type (single‑stitch | double‑needle | overlock), country_of_manufacture, blank_brand, blank_model, blank_year_codes (Hanes union tag = pre‑1985 etc.).
B.2. Accessories (P0)
B.2.1 Pins, Patches & Soft Goods

Enamel pins: hard enamel, soft enamel, cloisonné, die‑struck, screen‑printed, glitter‑filled, glow‑in‑the‑dark, sliding/spinning/kinetic, dangling, 3D, blind‑bag/mystery.
Lapel pins, button pins (pinback / pinbacks 1"/1.25"/1.5"/2.25"/3"), magnet pins, locking pin backs (rubber, metal deluxe).
Acrylic pins, wood pins, tin badges.
Patches: iron‑on, sew‑on, velcro/PVC tactical, embroidered, woven (higher detail), chenille, leather, sublimated, ribbon, back patch (separately flagged — battle‑jacket centric), sleeve patches, pocket patches.

Specific fields: pin_type, posts_count, backstamp, numbered_edition, edition_size, presentation (loose, on backer card, in plastic bag, in mystery bag, framed).
B.2.2 Carry
Tote bags (canvas, jute, mesh, designer), backpacks, slings, fanny packs / bum bags, crossbody bags, messenger bags, duffles, gym bags, dopp kits, weekend bags, suitcases (rare), pencil cases, lunch boxes (metal vintage, modern), bento boxes, cooler bags, drawstring bags, packable bags.
B.2.3 Tech accessories
Phone cases (per‑model: iPhone 6 → 17, Pixel, Galaxy lines), AirPods cases, AirTag holders, Pop sockets / phone grips, MagSafe accessories, charging cables (branded), USB drives (data + content), power banks, wireless chargers, laptop sleeves, laptop skins, keyboard keycaps (single, themed sets), gaming mousepads (small / desk‑mat / XXL), console skins (per‑gen: PS3/4/5, Xbox 360/One/Series, Switch, Switch 2, Steam Deck, ROG Ally), controller skins, headset stands, monitor stands, branded cable clips, branded webcam covers.
B.2.4 Drinkware
Mugs (ceramic, enamel, color‑changing, travel), tumblers (Yeti‑style stainless, Stanley‑style, character lid), water bottles (plastic, aluminum, glass, Nalgene, Hydroflask‑style), shot glasses, pint glasses, beer steins, wine glasses, champagne flutes, flasks, sake sets, bubble tea cups (recent K‑pop merch).
B.2.5 Smallgoods & EDC
Keychains (metal, rubber, acrylic, plush, lenticular, light‑up), keyrings, bottle openers, multi‑tools, lighters (Zippos with band logos), matches, ashtrays (historical), pocket knives (gated), playing cards (custom decks), dice sets (cross with tabletop), magnets (fridge), stickers (cross with paper).
B.2.6 Wallets & Personal
Wallets (bifold, trifold, card holder, money clip, chain wallet), passport holders, luggage tags, watch straps, watches (full‑on collab watches), jewelry (necklaces, chains, bracelets, rings, earrings — studs/hoops/dangles, anklets, body chains, grillz, religious/spiritual jewelry as merch, charms, lockets), beads (mardi gras‑style, festival), friendship bracelets.
B.2.7 Beauty / Personal Care
Perfumes / colognes (with named collab — Britney Spears Curious, etc.), lip balms, lipsticks, eyeshadow palettes, nail polishes, polish sets, false lashes, hair product, soap bars, bath bombs, candles (scented merch — Yankee Candle collabs, A24's Hereditary candle), incense, hand sanitizer (pandemic era flagged), sunscreen (brand collab), tattoo flash sheets, temporary tattoos, real tattoos appointment cards as memorabilia.
B.3. Collectibles (P0)

Action figures: by scale (1/12, 1/6, 1/4, 1:18 etc.), articulation count, accessories included; mfg: Hasbro, Mattel, NECA, Hot Toys, Mezco, McFarlane, Bandai SHFiguarts, Figma, Nendoroid.
Funko Pop! ecosystem: Pop, Pop Rides, Pop Town, Soda, Vinyl Soda, Mystery Minis, Bitty Pop; with vault_status, chase_variant, convention_exclusive (SDCC, NYCC, ECCC), retailer_exclusive (HT, Walmart, GameStop, BoxLunch).
Statues / busts: PCS, Sideshow, Iron Studios, Prime 1, Kotobukiya; resin vs polystone; LE size.
Plush toys: standard plush, jumbo (giant), beanie‑style, blind bag mini, weighted, talking, Build‑A‑Bear editions, Squishmallow style, kigu‑plush.
LEGO: set number, theme, pieces, minifigs included, year, retired_status.
Gashapon / Gachapon / Blind boxes: Pop Mart, Sonny Angel, Smiski, capsule sets, with series_completion_size and secret_chase flag.
Trading cards / TCGs: separate from sports cards in some scenes; capture set, set_code, card_number, rarity, foil/holo type, language, parallel/insert, signed, graded (PSA/CGC/BGS slab serial), grade.
Sports cards: brand (Topps, Panini, Upper Deck, Bowman), year, set, parallel, autograph, patch, /numbered.
Bobbleheads, snow globes, music boxes, wax figures (mini), crystal/glass figurines, model kits (Gunpla scale, Bandai numbering), die‑cast vehicles (1/64 Hot Wheels, 1/18, 1/24).

B.4. Paper & Print (P0)
Posters (offset, screen print, lithograph, giclée, foil variant, glow variant, blacklight, artist‑signed/numbered, AP — artist proof, PP — printer proof, Mondo style with Mondo number), tour posters (date/city specific, edition size on the bottom), gig flyers, art prints, lithographs, books (artbooks, photobooks, biographies, coffee‑table, tour books, tour programmes), magazines (official tie‑in or featured cover issue), comics tie‑in, calendars, postcards, greeting cards, bookmarks, sticker sheets, sticker packs, die‑cut stickers, vinyl stickers, holographic stickers, washi tape, stamps (postage merch — Elvis stamps), notebooks, journals, sketchbooks, planners, day‑planners, tarot decks, oracle decks, zines (size: ¼ letter, half letter, mini), chapbooks, blueprints (movie production blueprints), cels (animation), storyboards, film strips, lobby cards, press kits, EPKs (digital archived).
Field add‑ons: print_method, paper_stock_gsm, numbered_xx_of_yy, signed_by[], chop_marks, embossed: bool.
B.5. Music‑specific physical (P0)
Vinyl records: 7", 10", 12", LP, EP, picture disc, splatter, marbled, color, color‑in‑color, smoke, half‑and‑half, etched, lathe cut, flexi disc, shaped picture disc, RSD/BF‑RSD edition, 180g/200g; matrix/runout codes are a metadata field. Cassette: standard, gold tape, metal IV, shell color, J‑card variants, pro‑dub vs CDR. CDs: jewel, digipak, ecopak, slipcase, longbox vintage, mini‑LP replica, SACD, HDCD, mini‑disc, 8cm CD‑single, CD+Blu‑ray combo, dualdisc. USB stick releases. SD card releases. Music boxes with songs. Sheet music, songbooks, scores, set lists (signed/framed), guitar picks (standard, engraved, shaped, signature), drumsticks (standard, signed, broken on stage), drumheads (signed), guitar straps, instrument stands, custom pedals (collab), tour laminates, backstage passes (all‑access, working, photo, day‑specific), VIP package contents, fan club kits.
B.6. Home & Living (P1)
Tapestries (per‑size), wall flags, posters framed, canvas prints, throw blankets, fleece blankets, weighted blankets, pillowcases, cushion covers, full bedding sets, towels (bath, hand), bath mats, shower curtains, kitchen aprons, oven mitts, placemats, table runners, coasters, cutting boards, candles, diffusers, room sprays, incense burners, lamps & nightlights, neon signs (officially licensed), clocks (wall, alarm, cuckoo), barometers, doormats, rugs (small to room‑size), couch cushions, floor cushions, bean bag chairs, branded furniture.
B.7. Gaming / Tech‑specific (P1)
Steelbooks, collector's editions (with full BOM list — every component a sub‑item), strategy guides, soundtracks (cross‑linked to music), figures bundled, replica props, prop weapons (display only), code redemption cards (archived), gaming chairs, gaming desks, branded peripherals (mice, keyboards, headsets — collab limited), Steam Deck/console limited consoles, controllers limited, Amiibo, Skylanders, Disney Infinity figurines.
B.8. Event & Experience (P0)
Concert tickets (paper, RFID wristband, e‑ticket screenshot — metadata for date/venue/section), festival passes, VIP laminates, backstage passes, meet‑&‑greet photo prints, Polaroids with artist, signed photos, photo‑op prints (K‑pop fansign), event‑exclusive merch flagged (e.g., "Coachella W1 only"), pop‑up shop exclusives, merch‑truck‑only items, listening party giveaways, residency exclusives (Vegas).
B.9. Digital & Virtual (P1 — archive only, no ownership transfer)
NFTs (archived as historical artifacts; we never list price/transact), digital wallpapers/screensaver, avatar items (Roblox/Fortnite/VRChat), Discord boost perks (server badges), Twitch channel‑points redemptions, digital art files, virtual concert tickets (Travis Scott Fortnite event), DLC skins, in‑game item codes.

Marketplace‑readiness rule: the data model stores is_transferable: bool, transfer_method (physical | digital_code | digital_NFT | non_transferable), even though the marketplace is unbuilt. (P0 schema, P3 surface.)

B.10. Food / Consumables (P1)
Limited‑edition energy drinks (G FUEL flavors), branded cereal (Travis Scott Reese's Puffs), candy collabs, hot sauces, coffee blends, tea sets, branded wines/beers (Iron Maiden Trooper, Megadeth), instant noodles (Pewdiepie ramen), chocolate bars. Note: physical condition expires; we mark is_perishable: true and condition grading uses a different sub‑scale ("sealed factory" / "opened" / "consumed‑empty‑container only").
B.11. Pets (P2)
Pet collars, harnesses, bandanas, pet costumes, pet beds, pet bowls, pet toys, leashes, ID tags branded.
B.12. Sports memorabilia (P0 — sport‑heavy)
Game‑used equipment (jersey, ball, stick, glove, shoe, bat, helmet, mouthguard), match‑worn (vs. game‑issued vs. game‑used distinction), pennants, foam fingers, rally towels, branded scarves (soccer ultras), team flags, signed memorabilia, Olympic torches, medals (replica vs. authenticated), trophies (replica), official match programs, ticket stubs as collectibles.
B.13. Unique / 1‑of‑1 / Stage‑used (P2)
Stage‑worn clothing (with stage_used: true, event_id), instruments played on stage / on a record, tour vehicle parts, rigging elements, set‑piece fragments, stage props, drum heads, smashed guitar fragments (Pete Townshend lineage), worn‑on‑film costumes (with screen‑match documentation), production crew jackets (limited issue), road‑case stencils, tour bus signage.

Each 1‑of‑1 item carries chain_of_custody[] log.


C. Community Features (P0–P2)
C.1. Identity & social graph (P0)

Profiles with avatar, banner, location (country level by default), pronouns optional, bio, "top 4" pinned items (Letterboxd‑inspired), top universes, primary collecting era, joined date, last seen, badge wall. Wikipedia
Follow / Mutual / Block / Mute / Report primitives.
Friends as a higher‑trust tier (mutual follow + opt‑in) granting features like activity diff, friend‑only lists, trade visibility (later), and gift wishlist visibility.
Reading list of users/lists/items (separate from collection).
Profile privacy settings: public / signed‑in‑only / friends / private; per‑section overrides (collection vs wishlist vs diary).

C.2. Activity, feeds & notifications (P0)

Activity stream (chronological + algorithmic toggle).
Filter: my friends / global / specific universe / specific entity / specific tag.
Verbs: added to collection, wishlisted, reviewed, rated, photographed, edited (wiki), commented, attended event, drew fan art, hunted grail, completed set.
Per‑verb notification toggles. Daily digest email opt‑in.
"What's new in your fandom" digest (P1) — weekly auto‑newsletter per universe you follow.

C.3. Reviews, ratings, diary (P0)

5‑star with halves on items (Letterboxd‑style) and on the underlying source release. Letterboxd
Long‑form review (markdown, image embed, embed of other items).
Acquisition diary: log when/where/how you got an item (date, venue, price, purchased/gifted/found/inherited/won/traded). Diary can be private.
Re‑logs allowed (for "I owned this twice in my life").
"I've held this in my hands" log (without owning) — for visited‑museum / friend's‑collection items (P1).

C.4. Lists (P0)

Free‑form item lists with title, description, ranked or unranked, themed cover, collaborator support (multi‑user editable, P1).
List of lists (meta‑lists).
Cloning lists (RYM/Letterboxd Pro idiom). Letterboxd
Auto‑lists (smart lists driven by filters: "All AOP shirts from Japanese metal bands 2005‑2015").
List comments, likes, saves.

C.5. Grail hunting (P1) — new

Public wishlist with priority (low|med|high|HOLY GRAIL) and desired_condition_floor.
Grail card displays "X users helping hunt" who clicked "Help find this".
When a community member documents the item somewhere (e.g., a sold listing observed elsewhere, an archived store page), they can "Tip" the hunter, generating a notification.
Public Grail leaderboard per entity ("Most‑wanted Iron Maiden 1984 Powerslave longsleeve").
Grail fulfilled badge for the user when they finally acquire it; fulfilled_at timestamp; the hunt thread becomes a celebrated archive post.

C.6. "First seen" credit system (P1)

Every item has first_documented_by_user_id permanently displayed and contributing to user archivist score.
Users get badges: "First Documenter ×10/×100/×1000".
Editorial governance: rare cases where credit is contested go through admin review with first_documented_disputes table.

C.7. Collective memory & event linking (P1)

Items can be linked to specific event instances: tour date, festival night, pop‑up window, convention day.
"I was there" check‑in: users mark themselves as attending event; items they own from that event surface a chip "Was there: yes / linked".
Event memory wall: the event page aggregates all owners + their "I wore this to" + photos at the event.
Cross‑links to setlist.fm / Songkick / Bandsintown for music events (when feasible) — read‑only.

C.8. Drop calendar & alerts (P0–P1)
See Section I.
C.9. Authentication & counterfeit detection (P1)

Crowdsourced: any item can be flagged bootleg | suspected_bootleg | reprint | unofficial_repro | authentic.
Bootleg explainer thread: experts annotate distinguishing tells (single‑stitch only on hem, wrong tag font, etc.) with photo overlays.
Authentication levels:

L0 community vote.
L1 verified expert (badge holder) endorsed.
L2 brand/artist confirmation (when verified entity weighs in).
L3 third‑party graded (PSA/CGC slab serial linked).


Each item has an "Authentication tab" with public history.

C.10. "I wore this to / used this at" (P1)

Per‑owned item, link to one or more events. Powers the Passport (Section J) and the venue/event memory pages.

C.11. Fan art gallery (P2) — sister system

Users upload fan art inspired by a merch design or an entity. Linked back to the source merch.
Distinct from item photos. Has its own moderation lane (copyright sensitive).
Optional "remix tree": fan art referencing fan art referencing official.

C.12. Merch design lineage (P1) — new

Merch items can declare references[] (artworks, album covers, prior merch, films, paintings, memes).
Visual graph view: nodes are designs, edges are references; surface as "Lineage" tab on item.
Powers "Merch family trees" (D.6).

C.13. Sound‑linked items (P2)

Items shipped with a download/streaming code (vinyl insert codes, QR codes, mixtape USB) are flagged sound_linked: true. The associated audio metadata (release id, track list) is stored, so collectors can search "items that originally came with audio you can no longer get."

C.14. Stories (long‑form acquisition essays) (P1) — new

A separate text type from "review": Story. Long markdown, multi‑image, attached to a collection entry.
Story can be co‑authored (e.g., two people went on the trip).
Stories surface on the item page in a "Stories" tab and on user's profile under "Memoirs".

C.15. Bucket lists (P2)

Public "want to own before I die" lists, capped at 25 items, encouraged to be intentional.
Annual reflection prompt.
Community can fulfill via gifting (later marketplace tie‑in).

C.16. Documentation challenges (P1)

"Well‑Documented" certification: an item entry that meets photo standards (front, back, tag close‑up, neckline, hem, print detail, lighting standard) earns the "Well‑Documented" badge.
A community photo‑certifier role can endorse items.
Items below standard get a ghost overlay urging contributors to improve.

C.17. Size & fit notes (P1)

Per item type: aggregated community votes "Runs small / true / large", "Boxy / fitted", "Long body / short body".
Owners can submit measurements (pit‑to‑pit, length, sleeve) — averaged, with outliers flagged.
Chest‑to‑length ratio chart auto‑generated for tees.

C.18. Collaboration radar (P1)

Brand A × Brand B × Artist C — any 2+ entities = a "collab".
Collab pages aggregate every item under the collab.
Filter the universe browser by "collabs only."

C.19. Era tagging (P0–P1)

Years are structured.
Fan‑defined eras are a folksonomy layer per entity: e.g., Taylor Swift's "Lover Era," "Reputation Era"; K‑pop "comebacks" by name + date range; metal "early growl era" etc.
Era tags are versioned, voted, can fork.

C.20. Lore / cultural significance notes (P1)

Wiki‑style markdown field on items. Versioned, attributable.
"Why does this item matter?" template prompts.

C.21. Regional exclusivity & language (P0)

Hard fields: region_exclusive_to[], tour_leg, venue_id, language_of_print[].
Filterable.

C.22. Discussion & forums (P1)

Per‑entity discussion board.
Per‑item comment thread (already in v1, deepen with threading + reactions).
Per‑universe forum for general talk.
Tag‑specific feeds.
"Question" post type ("Anyone seen this tag style before?") with accepted‑answer.
Q&A reputation (Stack Overflow‑light).

C.23. Direct messaging & group DMs (P1)

1:1 + small group (≤ 10).
Photo sharing for ID help.
Trade negotiation hooks (data model only, P0 schema).

C.24. Reactions (P0)

Like + small reaction palette: 🔥 grail, 👀 want, ✋ have, 📚 learned, 🧪 expert‑seal.
Per‑content type tuning of available reactions.

C.25. Notifications inbox (P0)

Bell with grouped notifications.
Categories: replies, mentions, follows, drops, grail tips, your edits accepted/rejected, badge unlocks.

C.26. Mentorship & curator program (P2)

Power users can apply for "Curator" status per universe; granted edit‑lock authority on certain entity pages, plus weekly featured‑review slot.


D. Unique Differentiators (P1–P3)
D.1. Cross‑fandom discovery engine (P1)

"Fans of [X] also collect [Y]" — collaborative filtering on collection vectors.
"Surprise me" mode — daily cross‑fandom recommendation: a Sailor Moon collector gets shown a Bauhaus poster because of demographic overlap.
Privacy: opt‑in to be included in the recommendation graph.

D.2. Completionist tooling (P0)

Per release / collection / set: progress bar (e.g., "12/24 Madonna Confessions Tour shirts archived in your collection").
"Complete the set" suggestions.
Set‑definition is community‑editable; users can "fork" a set definition.

D.3. Merch archaeology mode (P1) — new

Mode applied to defunct artists/brands.
Page surface: "Reconstructing the merch of [entity]" with a stretched timeline, gaps marked in red (years where no merch is known), prompting community to fill.
Auto‑detected gaps: years with active touring but no documented item.

D.4. Ghost items (P2) — new

Items the community believes existed but has never photographed.
Submitter posts a "Ghost claim" with sources (forum posts, low‑res photos, sworn testimony).
Page status: ghost (unresolved) → summoned (definitive photo found, often the most celebrated event on site).
Quest leaderboard.

D.5. Museum mode (P1) — new

A read‑only beautified presentation of any user's collection (or sub‑collection / list).
Two preset themes:

Gallery white‑cube: spaced, captioned, minimalist, item‑by‑item walkthrough.
Cabinet of curiosities: dense, Victorian, layered.


Public sharable URL archivingmerch.com/museum/<user>/<collection>.
Visitor counter, guestbook (P2).

D.6. Merch family trees (P2)

Auto + manual lineage graph (see C.12).
Visual: D3‑style force layout (modern theme) / "Windows Explorer tree" view (XP theme — see Section E).

D.7. Canonical items (P2) — new

Per entity, the community votes on "The 5 canonical pieces" every fan must own — recomputed quarterly.
Discussion thread per nomination.
Merch is awarded a "Canonical" rosette.

D.8. Living archive — last‑seen tracking (P1)

For items still in production: last_seen_in_stock timestamp, with channel where seen (URL kept private).
Auto‑detects "Possibly sold out" if no stock confirmation in 30/90 days.
"Restock alerts" via watchers count.

D.9. Merch DNA — cultural moment linking (P2)

Items get a cultural_moment[]: links to album release, world event, meme, season finale, sports victory, scandal.
The site renders a small "DNA strand" graphic on item pages: artistic moment + cultural backdrop.
Powers a "Time machine" landing page: "The merch of November 2001."

D.10. Naming rights for fan‑documented items (P3)

The first documenter of a previously unknown variant gets to propose a community nickname (e.g., "the Cigar Tag," "the Glow Run," "the Misprint Eyebrows").
Community votes; once accepted, the nickname is canonical metadata.

D.11. Time machine page (P2)

Pick a year/month → see the merch landscape: top added items from that period, biggest tours, cultural moment linkage.

D.12. Visual similarity search (P2)

Upload a photo of merch you found at a thrift store → vector‑search the archive for similar designs. Useful for ID help.

D.13. AI‑assisted contribution (P1, opt‑in)

Optional vision model suggests fields on upload: detected type (tee/hoodie), guessed era, OCR of tag, suggested entities — user must confirm. AI never auto‑accepts.

D.14. "Ratio of survival" stat (P3)

For limited editions, displays "edition size 500 → currently documented 37 → ~7.4% surfaced." Drives both awe and discovery.


E. Dual Theme System — UI/UX Specification
The site ships two co‑equal themes. Neither is the "default." On first run, prompt user to pick (with previews); store choice; offer per‑device override; auto‑switch by time‑of‑day option.
E.1. Design tokens shared (P0)
Both themes share the same component library and semantic tokens (color.surface.primary, color.text.body, radius.card, space.sm, etc.). The token values differ. This guarantees identical IA and content density.
E.2. Theme A — Windows XP "Luna" Nostalgic (P0)

Window chrome: title bars with gradient blue (#2A6CD3 → #4A8AE8), white text, classic close/minimize/maximize buttons, draggable windows for "Power user" desktop mode.
Two layout modes:

Document mode (default): linear scrolling, single window framing the content, taskbar at bottom (sticky), Start button = primary nav.
Desktop mode (opt‑in, P1): multiple resizable windows on a teal‑hill wallpaper; users can "minimize" their collection while reading a review; persistence of window positions per user.


Start menu: my profile, my collection, my wishlist, browse universes, settings, help, log off.
System tray: notifications icon, radio player icon, clock, language toggle.
Icons: custom 16×16 / 32×32 pixel‑style for every universe (e.g., music = mini cassette).
Fonts: Tahoma 11px primary, Trebuchet MS for headings, Tahoma Bold for emphasis. (Tahoma is a XP signature.)
Buttons: beveled 3D, gray gradient, with focus blue dotted outline.
Form controls: classic Windows checkboxes, radio buttons, dropdowns, native‑feeling.
Tooltips: pale yellow (#FFFFE1) with 1px black border.
Dialog patterns: modal "Are you sure?" with proper "OK / Cancel / Apply" footer.
Cursor: custom XP arrow + "wait" hourglass on heavy actions (pure CSS).
Sounds (opt‑in default OFF, P1): chime on notification, error ding, "Tada!" on badge unlock — all derived/original (no IP).
Easter eggs (P1):

Solitaire mini‑game launchable from Start > Games.
Clippy parody mascot ("Maxxy the Merch Cat"?) suggesting tips, dismissable forever.
Rare BSOD on April 1 with a fake error then graceful auto‑recover.
Bliss wallpaper hidden in About page.


Mobile XP mode:

Taskbar becomes bottom mobile nav (5 icons + Start).
Windows collapse into vertical sheets.
Title bars become section headers.
"Start menu" becomes a full‑screen overlay.



E.3. Theme B — Modern Dark "Obsidian" (P0)

Surface palette: background #0B0D10, surface #14171C, elevated #1B1F26; accent #7C5CFF (configurable accent: violet / cyan / magenta / amber).
Typography: Inter (system fallback: SF Pro / Segoe UI Variable) 14/16px body; tabular nums for stats.
Layout: sidebar navigation (left, collapsible), top utility bar (search, notifications, user), content canvas. Mobile: bottom tab bar.
Cards: subtle borders #23272F, soft glow on hover, rounded 12px.
Motion: 120ms ease‑out micro‑interactions, spring on drawer open, no parallax abuse.
Density toggle: Compact / Comfortable / Spacious.
Image presentation: large hero, edge‑to‑edge gallery, dominant‑color extracted background tint per item.
Charts (passport, completionism, stats): clean line/bar/sparkline, animated reveal.
Easter eggs:

Konami code triggers the radio player with a hidden playlist.
Long‑press on logo: "Theme: Obsidian" tooltip with a tiny credits scroll.
Hidden "Light" theme stub for accessibility/laptop‑in‑sun (P3 — explicitly de‑prioritized; the two main themes lead).



E.4. Components in both themes (mappings)
ComponentXP renderingModern renderingItem cardBeveled box, blue title bar with item name, content area beigeFloating dark card with hero image, dominant‑color glowModalClassic Windows dialog, title bar, OK/CancelSheet with backdrop blur, primary action accentTabsXP tab strip, gray with selected white tabUnderline tabsDropdownClassic comboboxFloating menuToast"Balloon tooltip" near taskbarTop‑right slide‑in cardLoadingHourglass cursor + classic blue ProgressBarSkeleton shimmerFamily treeWindows Explorer tree (folder icons, +/− expanders)Force‑directed graph (D3)
E.5. Theme switch UX (P0)

Setting in account preferences with side‑by‑side preview cards.
"Try" mode without committing.
Per‑device override (so phone can be modern, desktop XP).
API: prefers-color-scheme ignored as authoritative (we ask the user actively); but a Modern sub‑variant respects light/dark when implemented.

E.6. Mobile parity (P0)
Both themes must achieve full feature parity on mobile. Differences:

XP mobile leans into "PocketPC 2003" aesthetic for charm.
Modern mobile is iOS/Android native‑feeling, gesture‑rich.

E.7. Accessibility (P0)

Both themes meet WCAG 2.2 AA at minimum.
XP theme: provide user setting to disable beveled gradients in favor of higher‑contrast borders without leaving the XP feel.
Both: respect prefers-reduced-motion. XP "minimize" animation skips on reduce.
Keyboard navigation parity.
Screen‑reader landmarks identical across themes; only visual layer differs.

E.8. Theme‑specific personality (P1)

XP error states mention "An unexpected error has occurred. Click OK to continue." with whimsical detail.
Modern errors are terse and pragmatic.
XP onboarding wizard literally calls itself "Setup Wizard."
Modern onboarding is a sleek 4‑slide swipe.


F. Merch Condition Grading System
F.1. Why a merch‑native scale (P0)
Existing scales (Discogs Goldmine for vinyl, CGC 10‑pt for paper, PSA for cards, Grailed/vintage for clothing) are each domain‑specific. We synthesize a two‑axis model:

A. Universal Condition Letter Grade (subjective, fast, like Grailed/vintage).
B. Optional Numeric Sub‑Grade (strict, comparable, like CGC/PSA).

F.2. Universal Letter Grade (P0)
CodeNameDefinitionDSDeadstock / NWTNever used, original tags / shrinkwrap intact, original packaging present. Savvy Row The "factory sealed" state.NMNear Mint / NWOTNew without tags; appears unused; has been stored carefully. Vintage Wholesalers No visible flaws.EX+Excellent PlusLight use; minor handling marks; no fading; structurally perfect.EXExcellentWorn/used a few times; light wash wear; no flaws to the casual eye. Savvy RowVG+Very Good PlusVisible wear consistent with age; no holes, no major stains; design fully intact.VGVery GoodNoticeable wear, minor stains/fades possible; design legible; structurally sound. TerravintageclothingGGoodSignificant wear; small holes/stains/fading; still wearable/displayable. MadgeshatboxFFairMajor flaws; piece needs care; collectible mainly for rarity. MadgeshatboxPPoorHeavily damaged; archival/historical interest only. VintagevaluesboutiqueRRestorationHas been restored/altered; restoration scope must be detailed.

Modeled on Grailed's vintage convention + Goldmine + Madge's Hatbox + Vintage Vixen, harmonized.

F.3. Numeric Sub‑Grade 1.0 – 10.0 (P1, optional)
10‑point, half‑points allowed. Map onto letters approximately:

10.0 = DS (perfect, magnification‑clean)
9.5–9.9 = NM
9.0 = EX+
8.0 = EX
7.0 = VG+
6.0 = VG
4.0–5.0 = G
2.0–3.0 = F
1.0 = P
0.5 (Authentic Altered) = R

F.4. Type‑specific addenda (P0)

Apparel: check 9 zones (front print, back print, neck tag, hem, side seams, sleeves, pits/stains, fabric pilling, hardware). Photo‑evidence template.
Vinyl/CD/Cassette: dual grade — Sleeve / Media (Goldmine convention preserved).
Pins/patches: front / back / posts.
Cards: centering / corners / edges / surface (4 sub‑grades, BGS‑style). Winners and Whiners
Posters: corners / surface / fold lines / pinholes / restoration.
Plush / figures: in‑box vs out‑of‑box; box condition graded separately.
Perishables (food): sealed / opened / consumed.
Authenticity flag is separate from condition (a fake can be NM).

F.5. Provenance flags (P0)
signed_by[], signed_provenance (witnessed | COA | unverified), stage_used, screen_used, match_used, match_worn, tour_used, production_use. Multiple allowed.
F.6. Community grading workflow (P1)

Users self‑grade.
Other users can suggest a re‑grade with photo evidence; original owner accepts/rejects.
Top reviewers earn "Trusted Grader" badge after N corroborated grades.
Items with grading disputes are flagged on the page until resolved.

F.7. Storage method tag (P1)
stored_in: archival sleeve, frame UV‑filtered, polybag, hung, folded, climate‑controlled, on display. Affects grade trajectory recommendations.

G. Tags & Folksonomy
A multi‑axis tagging system. Each axis can be free‑form and curated. Tags are versioned, voted, can have synonyms; a moderation team prevents tag fragmentation.
G.1. Era tags (P0)

Decade: 60s, 70s, 80s, 90s, y2k, 2000s, 2010s, 2020s.
Micro‑era: early-90s, peak-y2k (2001-2003), vaporwave-era (2011-2015), pandemic-era (2020-2021).
Cultural era: pre-9/11, post-9/11, great-recession, tumblr-era, streamer-era, tiktok-era.
Technology era: cassette-era, compact-disc-era, early-internet, web-2.0.
Fan‑defined eras (per entity): see C.19.

G.2. Style/aesthetic tags (P0)
bootleg-aesthetic, underground, mainstream, crustcore-aesthetic, cybercore, metalcore-graphic, gorgonart, risograph, dye-sublimation, airbrush, glitter-print, puff-print, flock-print, rhinestone, tie-dye, acid-wash, bleach-distressed, intentionally-distressed, gothic-lettering, serif-tour, varsity-style, streetwear, workwear, outdoor, military-surplus, cottagecore, vaporwave, seapunk, sleazecore, kawaii, decora, lolita, harajuku, K-pop-fanmade-aesthetic, WW2-vintage.
G.3. Manufacturing & material tags (P1)
screen-print, DTG, puff-ink, discharge-ink, metallic-ink, glow-ink, embroidered, chenille, applique, single-stitch, double-needle, selvedge, garment-dye, pigment-dye, oversized-cut, boxy-cut, cropped-cut, tagless, relabel, union-tag, made-in-USA, made-in-Mexico, made-in-Bangladesh, made-in-Honduras, made-in-Pakistan, Hanes-Beefy-T, Anvil, FOTL, Gildan-2000, Tultex, Velva-Sheen, M&O-Knits, Champion-reverse-weave.
G.4. Cultural significance tags (P1)
canonical, iconic, fashion-week-shown, awards-show-worn, artist-favorite, band-canon, controversy-attached, recalled, lawsuit-attached, cease-and-desisted, removed-from-store.
G.5. Collector value tags (P0)
grail, holy-grail, sleeper, under-the-radar, common, mass-produced, limited, numbered, one-of-one, prototype, sample, factory-second, misprint, error, friends-and-family, promo, not-for-resale, internal-only, crew-only.
G.6. Provenance tags (P0)
tour-exclusive, venue-exclusive, tour-leg-exclusive, web-store-only, fan-club-only, members-only-newsletter, Japan-only, Korea-only, Europe-tour-only, Latin-America-only, RSD, Black-Friday-RSD, BF-RSD, Cassette-Store-Day, Free-Comic-Book-Day, convention-exclusive, SDCC-exclusive, NYCC-exclusive, Anime-Expo-exclusive, Wondercon-exclusive, Comiket-exclusive, pop-up-exclusive, listening-party-giveaway, pre-order-bonus, retailer-exclusive (Hot Topic, BoxLunch, GameStop, Target, Walmart, FYE, AS Colour, Ktown4u).
G.7. Condition‑specific tags (P0)
Complement the grading scale: pit-stain, bleach-spot, paint-spot, pinholes, tears, screenprint-cracking, DTG-fade, vintage-shrunk, frayed-hem, broken-zipper, re-screenprinted, re-printed-tag.
G.8. Content & sensitivity tags (P0)
nsfw, gore, nudity, sexual-content, profanity, religious-imagery, political-historical, tobacco-imagery, alcohol-imagery, drug-imagery, weapon-imagery, controversial-symbol. Drives content gating.
G.9. Audience / demographic tags (P1)
adult, unisex, women's, men's, youth, kids, infant, pet.
G.10. Tag governance (P0)

Synonym engine (tee ⇄ t-shirt).
Translation pairs FR ⇄ EN at tag level.
Anti‑fragmentation: tag autocomplete with "similar exists" suggestions.
Moderator panel to merge tags, deprecate, redirect.
Per‑tag landing page with top items, recent additions, top contributors.


H. Creator / Brand Verification
H.1. Goals (P0)

Trust signal to community.
Empower official entities to seed authoritative metadata.
Future: licensing/marketplace partnerships.

H.2. Verification tiers (P1)
TierBadgeEligibilityGranted byOfficial Entity (Verified)✅ blueThe legal/cultural rights holder of the entity (band, brand, athlete, creator, estate)Admin, with documentary proofOfficial Representative🟢 greenManager / label / agency rep speaking on behalfAdmin, with chain‑of‑rep proofEstate / Legacy Holder🟪 purplePosthumous estate, foundation, museum trustAdminSubject Matter Expert🟡 yellow starTop community contributor with deep universe expertiseCurators + adminTrusted Contributor⬡ outlinedFree contribution permission (no validation queue)Earned via track record
H.3. Verification process (P1)

Self‑claim from entity page → "Claim this profile."
Identity proof: choice of (a) link from official site to a verification token URL, (b) a verified email at the official domain, (c) DM proof from a verified social channel, (d) DocuSigned legal letter.
Admin review (SLA 5 business days).
Optional: video call for high‑value entities.
Granted with cosmetic badge + permissions.

H.4. Verified perks (P1)

Pin a "Welcome / Bio" message on entity page.
"Authoritative" toggle on metadata edits — bypass community vote (trackable).
Endorse community‑contributed items as official ("✅ confirmed by artist").
Drop calendar privileges (post their own upcoming drops without moderator queue).
Custom entity page banner + theme colors.
Direct fan inbox with rate‑limit (P2).
Analytics: who follows, top‑collected items.
"AMA" event scheduling (P2).
Right to flag bootlegs as such with priority moderation.
Ability to designate "canonical" items themselves (with disclosure that this is the artist's pick).

H.5. Anti‑abuse (P0)

Loss of verification on misuse.
Public log of badge grants/revocations.
Community appeal channel.
Verified entities cannot delete inconvenient archival entries (only flag for context).

H.6. Brand engagement loop (P2)

"This week in X fandom" newsletter sent by verified entities.
Sponsored/partnered drops (clearly disclosed, no algorithmic preference).
Future: official drops sold via the marketplace with a verified seal.


I. Events & Drops Calendar
I.1. Data model (P0)
events table: id, type (drop | tour_date | pop_up | release | anniversary | convention | restock | listening_party), title, entity_ids[], start_at, end_at (nullable), timezone, location {venue_id, online_url}, region_restrictions[], merch_links[], submitted_by, verification_status, source_urls[], rsvp_count, attendee_count.
I.2. Event types (P0)

Past drops (archival): backfill with timestamp + photos + reception notes.
Upcoming drops (community‑submitted, mod‑verified, verified‑entities post directly).
Tour dates: imported via Songkick/Bandsintown integration where possible (P1), or manually.
Anniversary drops (10‑yr re‑release shirts, etc.).
Collab announcements.
Pop‑up shop events (location + dates + items expected).
Listening party (often free‑merch giveaways).
Convention windows (SDCC pre‑sale window, etc.).
Restocks.
Recalls / pulled items.

I.3. Calendar UI (P0)

Modes: Month / Week / Day / Agenda / Map.
Filters: by universe, by followed entities, by region, by event type, by "free merch likely" boolean.
My calendar vs All calendar.
iCal feed export (P1).
"Add to my watchlist" → push notification N hours before drop.
"RSVP / I'll be there" with attendee count.
Post‑event recap cards: linked items, reviews, photos.

I.4. Drop alert system (P0)

User subscribes to entity → all drops surface.
Granular: "Notify only Japan‑exclusive," "Notify only ≤ $50," etc.
Channels: in‑app, email, optional browser push.

I.5. Tour merch prediction (P2)

For confirmed tours, machine‑learned guess of likely merch (based on prior tour patterns of the entity).
Marked as "Predicted" until items appear.
Encourages early venue check‑ins.

I.6. Moderation (P0)

Community submissions enter a queue.
Auto‑match against verified entity calendar to fast‑pass.
Source URLs required.
Hoaxes flagged → submitter trust reduced.


J. Merch Passport
J.1. Concept (P1) — unique differentiator
A passport‑style identity document for each user, auto‑populated from their collection + diary + event check‑ins. It is to a merch collector what Letterboxd's stats page is to a cinephile, but tactile, gamified, and journey‑oriented.
J.2. Visual design (P1)

Skeuomorphic passport cover (theme‑aware: XP renders a "Microsoft Passport"‑style document; Modern renders a sleek dark‑mode digital ID card).
"Pages" inside that flip/scroll.
Shareable as a beautiful image card (OG card for socials).

J.3. Pages (P1)

Identity page: avatar, "Citizen of [primary fandom]", member since, total items, total events attended.
Stamps page: country‑of‑origin stamps for every nation that produced merch in your collection. Stamp art per country curated.
Venues visited: every venue you've checked into.
Tour log: tours you have at least one item from.
Era passport: timeline ribbon showing decades represented in your collection, with dominant universe per era.
Customs declarations: rare/grail items declared with their stories.
Visa stamps: badges (see K.x).
Passport family tree: who you collect from (top 10 entities).

J.4. Mechanics (P1)

Auto‑generated.
Manual curation toggle (user can pin items).
Privacy levels: public / friends / private.
Annual "yearly recap" passport snapshot (Spotify Wrapped energy).
Compare passports with friends ("Both visited the Lollapalooza '08 stamp").

J.5. Stamps catalog (P1)

Country stamps (240+).
Era stamps (decadal).
Genre/category stamps.
Special event stamps (RSD attendee, SDCC attendee).
Milestone stamps (100 items, 1000 items, completionist‑set #1).
Limited stamps (joined during beta = "Founder" stamp permanently).

J.6. Stamp acquisition rules (P1)

Triggered on data events.
Some require self‑attestation (event check‑in). Anti‑gaming throttles in place.
Verified stamps (admin/event‑confirmed) are visually distinct from self‑claimed.


K. Cross‑cutting Specs
K.1. Data model overview (P0)
Top‑level tables:
universes, sub_universes, entities, entity_relationships, source_releases (album, tour, event, season, edition), items, item_variants, item_photos, ownerships, wishlists, reviews, ratings, stories, lists, list_items, events, attendances, tags, item_tags, comments, reactions, follows, blocks, badges, user_badges, audit_log, edits (wiki versioning), edit_proposals, edit_votes, authentications, grail_hunts, grail_tips, ghost_items, transactions_placeholder (for future marketplace), reports, moderation_actions, notifications, sessions.
K.2. Wiki versioning (P0)
Every editable field on entities/items/lists is wikified: each save creates a new revision with editor_id, ts, diff, reason. Diff view, revert (mod), watch‑page (subscribe to changes).
K.3. Trust & permissions (P0)

Trust score per user: weighted by accepted edits, accepted submissions, badges, age, peer endorsements.
Thresholds unlock: instant submission (no queue), edit lock bypass, voting weight, curator nomination.
All thresholds visible to user; gamified progress bar.

K.4. Submission & moderation flow (P0)

New users: validation queue, P0 schema.
Trusted users: instant publication.
Flagged: parallel moderation queue.
Public moderation log (transparent like Wikipedia's history).
Per‑universe moderation teams.

K.5. Search (P0)

Federated: entities, items, lists, users, tags.
Filters: universe, era, region, type, tag, condition, edition size, price band (when marketplace), in‑collection / not‑in‑collection toggle (logged‑in).
Visual search (P2, see D.12).
"More like this" surface on every item.
Save searches → become smart lists.

K.6. Internationalization (P0)

Full FR/EN, prepared for ES/JP/KR/PT/DE/IT (P1).
Per‑item: original language preserved + translations layered.
Per‑review: mark language; Letterboxd‑like language filter.
Right‑to‑left readiness in CSS even before AR/HE ship.

K.7. Accessibility, performance (P0)

WCAG 2.2 AA.
LCP < 2.5s on item pages, INP < 200ms.
Image pipeline: AVIF + WebP fallback, responsive srcset, lazy load, decoding="async".
Vanilla‑JS routing minimizes hydration cost; Supabase Edge functions for cacheable views.
Aggressive client‑side cache for static entity metadata.

K.8. Radio player (existing) integration (P1)

"Now playing on the archive": users can attach the radio show to an item entry as ambient context (linking listening sessions to collection actions).
Radio "merch corner": auto‑surfaces 3 items from currently playing entity's catalog.

K.9. Gamification & badge wall (P1)

Hundreds of micro‑badges across themes:

Documentation: First, x10, x100, x1000, x10000.
Photo quality: Bronze / Silver / Gold / Platinum lens.
Universe completion: "Sailor Senshi" (all 5 senshi entities collected), "Discography Diver" (full discography of any one artist documented).
Behavior: helpful (received N peer thanks), conscientious (N corrections accepted), generous (N tips on grail hunts).
Era: 70s veteran, Y2K survivor, etc.


Anti‑metric‑gaming: no public leaderboards on raw item count alone; weighted scoring.

K.10. Reporting & safety (P0)

Report content: spam, harassment, copyright, NSFW unflagged, bootleg fraud.
Per‑user "block" reciprocally hides content (Letterboxd model).
Anti‑harassment in DMs: filter words, rate limits for unestablished accounts.
Strong NSFW gate; default off; per‑category preferences.
Doxxing/PII redaction policies.

K.11. Content licensing (P0)

User‑contributed photos: default CC BY‑SA 4.0 (TShirtSlayer‑style); optional all‑rights‑reserved.
Metadata: CC0 (Discogs‑inspired), to encourage reuse and trust.
Public data dump pipeline (P2).
Public read API (P2), API tokens, rate limits.

K.12. Ethical & policy guardrails (P0)

Hard reject: hate symbols, terrorist orgs, CSAM (zero‑tolerance), active doxxing.
Politically active items reflagged with case‑by‑case review.
Religious items respected; commerce/proselytization framing forbidden.
Indigenous cultural items: opt‑in advisory committee for sensitive listings.
Counterfeit goods documented as artifacts, but never legitimized as authentic; clear bootleg tagging required.

K.13. Marketplace‑readiness data model (P0 schema, P3 surface)

Item ownerships hold for_sale_state (none | listed | reserved | sold), asking_price, currency, condition_at_listing, accept_offers: bool, accept_trades: bool, region_will_ship_to[], escrow_pref. Inactive in v2 — ready for v3.
Reputation scaffolding: transaction_history, seller_rating, dispute_log. Inactive.

K.14. Analytics & insight pages (P1)

Per‑entity stats: total items, total owners, top countries of origin, era distribution, rarest item, most reviewed.
Per‑universe leaderboards (curated, weighted).
Personal stats: collection by category, by era, by country, by year acquired.
"Wrapped" annual recap (P1).

K.15. Mobile apps (P3)

v2 ships responsive web only.
v3 PWA‑first; native shells (Capacitor) for camera‑first contribution UX.


L. Onboarding & First‑Run UX
L.1. New user (P0)

Choose theme (XP / Modern), with full preview.
Pick 3‑10 universes you care about.
Pick 5‑20 entities to follow.
(Optional) Add your first item — wizard with photo capture, tag suggestions, category picker.
Reveal: "your passport has been issued."
Surface 1 grail‑hunt suggestion + 1 list to follow.

L.2. Returning user (P0)

Daily prompt rotation: "Document an item," "Endorse a peer's grading," "Vote on canonical," "Clear 1 wishlist item."
Streaks (light, opt‑out).

L.3. Empty‑state messaging (P0)

Each empty section has a friendly path forward.
XP empty states use playful Win98/XP tone.
Modern empty states are concise.


M. Open Questions / Roadmap Risks

Edit‑war prevention at scale (Wikipedia/RYM lessons): we will need a protected‑page mechanism for hot entities.
Photo‑rights complexity at marketplace launch (resellers using community photos): pre‑clarify license and watermark options.
Verified entity churn: artists die, brands change hands. Need a clean "transfer of verification" workflow.
NSFW economics: gating costs (age verification provider) — choose carefully (Yoti / Persona / Stripe Identity).
Bootleg ethics: TShirtSlayer normalizes documenting bootlegs; we should keep that ethos but with clearer flags than they have.
Politically sensitive history: keep an explicit case‑law thread of decisions taken (good for community trust).
K‑pop tribalism: design moderator structures so the K‑pop universe doesn't import its more toxic patterns.
AI‑generated content tagging: surfacing AI in merch design provenance (echoing Spotify's 2026 verified‑artist push around AI personas) — every item gets ai_involvement: none | partial | primary | unknown (P1).
Event verification at scale: scraping vs. crowdsourcing tradeoff.
Theme parity discipline: every new feature must ship in both themes simultaneously, or in neither — enforce at PR review.


N. Priority Roll‑up by Release
v2.0 (next launch — P0)

Universe taxonomy 1‑20 (with Memory and Adult universes gated and stubbed).
Base + per‑type item schemas.
Letter grading scale.
Tag axes with synonym engine.
Reviews, ratings, lists, diary, stories.
Activity feed, reactions, follow/block.
Drop calendar (basic).
Verification tiers and process.
Both themes feature‑complete with parity, mobile parity, accessibility.
FR/EN i18n complete.
Marketplace data model in place (no UI).

v2.1 — v2.4 (P1)

Numeric sub‑grade.
Grail hunting & tipping.
First‑documenter credit, badges.
Museum mode.
Merch family trees (manual lineage).
Passport v1.
Documentation challenges.
Size & fit notes.
Drop alerts and prediction stubs.
Stories, fan art gallery (P2).
Cross‑fandom recommender.

v3 (P2)

Marketplace surface.
Visual similarity search.
Time machine page.
Ghost items system.
AMA / verified‑entity events.
Public API & data dumps.
Mobile PWA / app shell.

Long horizon (P3)

Auctions / bidding.
Naming‑rights system.
Survival ratio analytics.
AR "try it on" / AR room for posters and tapestries.
Physical NFC tag program ("scan this item to verify on Archiving Merch").


O. Closing Notes
The opportunity Archiving Merch addresses is latent: every collector community already does most of this work, badly, in scattered tools. Letterboxd proved that a kind, well‑designed cataloging social network can become genuinely culturally important. Discogs proved that an exhaustive open database can sustain a marketplace. TShirtSlayer proved metalheads will photograph every shirt in a closet. RateYourMusic proved folksonomy at scale. CGC/PSA proved condition language earns its keep. K‑Collect and Bias Room prove that K‑pop's photocard economy alone could sustain a vertical.
The thesis of this spec is that the same small set of primitives — entities, source releases, items, ownerships, photos, tags, events, reviews, lists, badges, edits — when applied universally and surrounded by lovingly opinionated UI in two beloved aesthetics, can unify all of these communities. Build the primitives ruthlessly well; let community do the rest.
Fin de la spec v2.0. À vos clavi̇ers.
