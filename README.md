<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>3D Print Idea Generator</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            min-height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            padding: 20px;
        }

        .container {
            background: white;
            border-radius: 20px;
            padding: 40px;
            max-width: 600px;
            width: 100%;
            box-shadow: 0 20px 60px rgba(0, 0, 0, 0.3);
        }

        h1 {
            text-align: center;
            color: #667eea;
            margin-bottom: 10px;
            font-size: 2.5em;
        }

        .subtitle {
            text-align: center;
            color: #666;
            margin-bottom: 30px;
            font-size: 1.1em;
        }

        .idea-display {
            background: linear-gradient(135deg, #f5f7fa 0%, #c3cfe2 100%);
            border-radius: 15px;
            padding: 40px;
            margin-bottom: 30px;
            min-height: 200px;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            text-align: center;
            transition: all 0.3s ease;
        }

        .category {
            display: inline-block;
            background: #667eea;
            color: white;
            padding: 8px 16px;
            border-radius: 20px;
            font-size: 0.9em;
            margin-bottom: 15px;
            font-weight: 600;
        }

        .idea-text {
            font-size: 1.8em;
            color: #333;
            font-weight: 600;
            line-height: 1.4;
        }

        .description {
            margin-top: 15px;
            font-size: 1em;
            color: #555;
            line-height: 1.6;
            max-width: 500px;
        }

        .difficulty {
            margin-top: 15px;
            font-size: 0.95em;
            color: #666;
        }

        .difficulty-level {
            display: inline-block;
            padding: 4px 12px;
            border-radius: 12px;
            font-weight: 600;
            margin-left: 5px;
        }

        .easy { background: #d4edda; color: #155724; }
        .medium { background: #fff3cd; color: #856404; }
        .hard { background: #f8d7da; color: #721c24; }

        button {
            width: 100%;
            padding: 18px;
            font-size: 1.2em;
            font-weight: 600;
            color: white;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            border: none;
            border-radius: 12px;
            cursor: pointer;
            transition: all 0.3s ease;
            box-shadow: 0 4px 15px rgba(102, 126, 234, 0.4);
        }

        button:hover {
            transform: translateY(-2px);
            box-shadow: 0 6px 20px rgba(102, 126, 234, 0.6);
        }

        button:active {
            transform: translateY(0);
        }

        .placeholder {
            color: #999;
            font-style: italic;
        }

        @keyframes fadeIn {
            from {
                opacity: 0;
                transform: scale(0.95);
            }
            to {
                opacity: 1;
                transform: scale(1);
            }
        }

        .animate {
            animation: fadeIn 0.4s ease;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>🖨️ 3D Print Ideas</h1>
        <p class="subtitle">Get inspired for your next project!</p>
        
        <div class="idea-display" id="ideaDisplay">
            <p class="idea-text placeholder">Click below to generate a random 3D printing idea!</p>
        </div>
        
        <button onclick="generateIdea()">Generate New Idea</button>
    </div>

    <script>
        const ideas = [
            { category: "Home & Office", text: "Desk organizer with custom compartments", difficulty: "easy", description: "Design separate sections for pens, clips, and sticky notes to keep your workspace tidy." },
            { category: "Home & Office", text: "Phone stand with adjustable angle", difficulty: "easy", description: "Create a stand that lets you view your phone at the perfect angle for video calls or watching content." },
            { category: "Home & Office", text: "Cable management clips", difficulty: "easy", description: "Small clips that attach to desk edges to route and organize charging cables and wires." },
            { category: "Home & Office", text: "Custom drawer dividers", difficulty: "medium", description: "Modular dividers that can be arranged to fit any drawer size and organize various items." },
            { category: "Home & Office", text: "Wall-mounted key holder", difficulty: "easy", description: "A decorative hook system to keep your keys organized and always in the same spot." },
            { category: "Home & Office", text: "Headphone stand", difficulty: "easy", description: "An elegant stand to display and store your headphones when not in use." },
            { category: "Home & Office", text: "Plant pot with drainage system", difficulty: "medium", description: "A two-piece planter with a reservoir to catch excess water and protect surfaces." },
            { category: "Home & Office", text: "Bookend with unique design", difficulty: "medium", description: "Functional bookends with geometric or themed designs to add personality to your shelf." },
            
            { category: "Kitchen & Dining", text: "Cookie cutter in custom shape", difficulty: "easy", description: "Design cutters in any shape you want - perfect for themed parties or special occasions." },
            { category: "Kitchen & Dining", text: "Napkin holder with modern design", difficulty: "easy", description: "A stylish holder to keep paper or cloth napkins organized on your table or counter." },
            { category: "Kitchen & Dining", text: "Spice rack organizer", difficulty: "medium", description: "Tiered or rotating rack to display spice jars and make them easy to access while cooking." },
            { category: "Kitchen & Dining", text: "Bag clip with living hinge", difficulty: "medium", description: "Flexible clips that seal open food bags to keep contents fresh for longer." },
            { category: "Kitchen & Dining", text: "Measuring spoon set", difficulty: "medium", description: "Custom measuring spoons with clear markings for precise cooking and baking." },
            { category: "Kitchen & Dining", text: "Egg separator tool", difficulty: "easy", description: "A simple gadget that separates egg whites from yolks quickly and cleanly." },
            
            { category: "Tools & Gadgets", text: "Screwdriver bit holder", difficulty: "easy", description: "Organize all your screwdriver bits in one place with clearly visible slots for each size." },
            { category: "Tools & Gadgets", text: "Hex key organizer", difficulty: "easy", description: "A holder that keeps your hex keys sorted by size and always accessible." },
            { category: "Tools & Gadgets", text: "Paint brush holder", difficulty: "easy", description: "Keep brushes organized and protected with individual slots that prevent bristle damage." },
            { category: "Tools & Gadgets", text: "Wire stripper gauge", difficulty: "medium", description: "A guide tool that helps determine the correct wire gauge for stripping cables." },
            { category: "Tools & Gadgets", text: "Clamp for holding small parts", difficulty: "medium", description: "A spring-loaded or screw-type clamp designed to secure tiny components during work." },
            { category: "Tools & Gadgets", text: "Angle finder tool", difficulty: "hard", description: "A measuring device that helps you find and replicate angles for woodworking or construction." },
            
            { category: "Gaming", text: "Dice tower with custom design", difficulty: "medium", description: "A tower that randomizes dice rolls with your own decorative theme or game motifs." },
            { category: "Gaming", text: "Card game organizer", difficulty: "medium", description: "Custom insert for board game boxes to keep cards, tokens, and pieces perfectly organized." },
            { category: "Gaming", text: "Game piece holder", difficulty: "easy", description: "Small bowls or trays to hold tokens, meeples, or other small game components during play." },
            { category: "Gaming", text: "Controller stand", difficulty: "easy", description: "A display stand for game controllers that keeps them charged and looking great." },
            { category: "Gaming", text: "Custom dice set", difficulty: "hard", description: "Design and print your own polyhedral dice with unique numbering or symbols." },
            { category: "Gaming", text: "Miniature figure base", difficulty: "easy", description: "Custom bases for tabletop gaming miniatures with textured or themed designs." },
            
            { category: "Art & Decoration", text: "Geometric wall art", difficulty: "medium", description: "3D sculptural pieces with repeating patterns or shapes that create visual interest on walls." },
            { category: "Art & Decoration", text: "Vase with spiral pattern", difficulty: "medium", description: "A decorative vase featuring twisting patterns that look great and can hold flowers or dried arrangements." },
            { category: "Art & Decoration", text: "Picture frame with unique border", difficulty: "easy", description: "Custom frames with decorative borders, patterns, or themed elements for your favorite photos." },
            { category: "Art & Decoration", text: "Candle holder", difficulty: "easy", description: "Decorative holders for tea lights or pillar candles with interesting geometric or organic shapes." },
            { category: "Art & Decoration", text: "Lamp shade with pattern cutouts", difficulty: "hard", description: "A shade that projects beautiful patterns when lit, creating ambient lighting effects." },
            { category: "Art & Decoration", text: "Christmas ornament", difficulty: "easy", description: "Festive decorations in any shape or design to personalize your holiday tree." },
            
            { category: "Tech & Electronics", text: "Raspberry Pi case", difficulty: "medium", description: "A protective enclosure with ventilation and port access for your single-board computer." },
            { category: "Tech & Electronics", text: "Cable organizer box", difficulty: "easy", description: "A box that hides power strips and excess cables while keeping devices plugged in." },
            { category: "Tech & Electronics", text: "SD card holder", difficulty: "easy", description: "Protective case with slots to organize and store multiple SD or microSD cards safely." },
            { category: "Tech & Electronics", text: "USB hub stand", difficulty: "medium", description: "An elevated stand for USB hubs that makes ports easily accessible and looks clean on your desk." },
            { category: "Tech & Electronics", text: "Camera lens cap holder", difficulty: "easy", description: "A clip or case that keeps lens caps attached to your camera strap so they're never lost." },
            { category: "Tech & Electronics", text: "Soldering iron holder", difficulty: "medium", description: "A safe stand with heat-resistant areas to hold your soldering iron during electronics work." },
            
            { category: "Storage & Organization", text: "Pegboard accessories", difficulty: "easy", description: "Custom hooks, shelves, and holders designed to maximize your pegboard's organizational potential." },
            { category: "Storage & Organization", text: "Battery dispenser", difficulty: "medium", description: "A gravity-fed organizer that stores and dispenses batteries by size for easy access." },
            { category: "Storage & Organization", text: "Thread spool rack", difficulty: "medium", description: "Wall-mounted or standing rack to organize sewing thread spools with easy pull access." },
            { category: "Storage & Organization", text: "Stackable storage bins", difficulty: "medium", description: "Modular containers that stack securely and can be customized to fit your exact storage needs." },
            { category: "Storage & Organization", text: "Modular drawer inserts", difficulty: "medium", description: "Customizable compartments that snap together to organize drawers exactly how you want them." },
            { category: "Storage & Organization", text: "Wall-mounted magazine holder", difficulty: "easy", description: "Sleek holder that displays magazines or papers while keeping them organized and off surfaces." },
            
            { category: "Toys & Fun", text: "Fidget spinner", difficulty: "medium", description: "A spinning toy with bearings that provides satisfying tactile feedback and smooth rotation." },
            { category: "Toys & Fun", text: "Puzzle box", difficulty: "hard", description: "A mechanical box with hidden mechanisms and sequential steps needed to open it." },
            { category: "Toys & Fun", text: "Marble run track pieces", difficulty: "medium", description: "Modular track sections that connect to create custom marble runs with ramps and turns." },
            { category: "Toys & Fun", text: "Wind-up toy mechanism", difficulty: "hard", description: "A spring-powered mechanism that can make toys walk, spin, or move when wound up." },
            { category: "Toys & Fun", text: "Building blocks set", difficulty: "easy", description: "Custom interlocking blocks compatible with popular building toy systems or unique designs." },
            { category: "Toys & Fun", text: "Mini basketball hoop", difficulty: "medium", description: "A desk-sized hoop and backboard for shooting foam balls during breaks." },
            
            { category: "Mechanical", text: "Gear mechanism demonstration", difficulty: "hard", description: "Working gear system that shows how different gear ratios affect speed and torque." },
            { category: "Mechanical", text: "Spring-loaded mechanism", difficulty: "hard", description: "A device using springs to store and release energy for movement or launching objects." },
            { category: "Mechanical", text: "Door stop with rubber insert", difficulty: "easy", description: "A sturdy wedge with space for a rubber grip to hold doors open firmly without scratching floors." },
            { category: "Mechanical", text: "Hinge system for boxes", difficulty: "medium", description: "Print-in-place or assembled hinges that allow boxes and containers to open smoothly." },
            { category: "Mechanical", text: "Ratchet mechanism", difficulty: "hard", description: "A one-way mechanical system that allows rotation in only one direction with satisfying clicks." },
            { category: "Mechanical", text: "Snap-fit joints demonstration", difficulty: "medium", description: "Various joint designs showing how parts can connect without screws or glue using flexible tabs." }
        ];

        function generateIdea() {
            const display = document.getElementById('ideaDisplay');
            const randomIdea = ideas[Math.floor(Math.random() * ideas.length)];
            
            display.classList.remove('animate');
            void display.offsetWidth;
            display.classList.add('animate');
            
            display.innerHTML = `
                <span class="category">${randomIdea.category}</span>
                <p class="idea-text">${randomIdea.text}</p>
                <p class="description">${randomIdea.description}</p>
                <div class="difficulty">
                    Difficulty: <span class="difficulty-level ${randomIdea.difficulty}">${randomIdea.difficulty.charAt(0).toUpperCase() + randomIdea.difficulty.slice(1)}</span>
                </div>
            `;
        }
    </script>
</body>
</html>
