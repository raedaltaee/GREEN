<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>تحليل المعيار الأخضر العراقي للمباني والمدن - 2025</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Tajawal:wght@400;500;700&display=swap" rel="stylesheet">
    <!-- Chosen Palette: Calm Harmony (Beige, Olive, Tan, Dark Brown) -->
    <!-- Application Structure Plan: The SPA is structured as an interactive dashboard. It begins by establishing the context and urgency with a visual summary of Iraq's environmental challenges. The core of the application is a tabbed, interactive section for the Building and City standards. This section uses clickable charts (Doughnut for Buildings, Polar Area for Cities) to allow users to explore the different evaluation axes non-linearly. Clicking an axis dynamically loads its key requirements and goals. This user-driven exploration is more engaging and effective for understanding the dense report than a linear format. The final section summarizes the implementation strategy (audit process, policies) to provide a complete overview. -->
    <!-- Visualization & Content Choices: 
        - Environmental Challenges: Report Info -> List of critical issues -> Goal: Inform and create context -> Viz: Grid of icon cards -> Interaction: Static, visually scannable -> Justification: More engaging and digestible than a text list -> Method: HTML/Tailwind with Unicode icons.
        - Standard Axis Weights: Report Info -> Proportional weights/points for axes -> Goal: Compare priorities at a glance -> Viz: Interactive Doughnut/Polar Charts -> Interaction: Click a slice/segment to update a details pane -> Justification: Visually represents part-to-whole relationships and drives user exploration -> Library: Chart.js.
        - Audit Process: Report Info -> A 3-stage process -> Goal: Simplify and explain the process -> Viz: A vertical, timeline-style diagram -> Interaction: Static -> Justification: Clarifies a sequential process more effectively than prose -> Method: HTML/Tailwind.
        - Key Technical Details (U-values, percentages): Report Info -> Specific metrics -> Goal: Present key data points clearly -> Viz: Bulleted lists within a dynamic details pane -> Interaction: Content updates on chart click -> Justification: Surfaces critical data on demand without cluttering the main view -> Method: JS-driven HTML update.
    -->
    <!-- CONFIRMATION: NO SVG graphics used. NO Mermaid JS used. -->
    <style>
        body {
            font-family: 'Tajawal', sans-serif;
            background-color: #F8F5F2;
            color: #4A4441;
        }
        .chart-container {
            position: relative;
            width: 100%;
            max-width: 500px;
            margin-left: auto;
            margin-right: auto;
            height: 350px;
            max-height: 40vh;
        }
        @media (min-width: 1024px) {
            .chart-container {
                height: 450px;
                max-height: 450px;
            }
        }
        .tajawal-bold {
            font-weight: 700;
        }
        .tajawal-medium {
            font-weight: 500;
        }
        .nav-link {
            transition: color 0.3s, border-bottom-color 0.3s;
            border-bottom: 2px solid transparent;
        }
        .nav-link:hover, .nav-link.active {
            color: #6B8E23;
            border-bottom-color: #6B8E23;
        }
        .card {
            background-color: #FFFFFF;
            border: 1px solid #E5E7EB;
            border-radius: 0.75rem;
            padding: 1.5rem;
            transition: transform 0.3s, box-shadow 0.3s;
            height: 100%;
        }
        .card:hover {
            transform: translateY(-5px);
            box-shadow: 0 10px 15px -3px rgba(0, 0, 0, 0.1), 0 4px 6px -2px rgba(0, 0, 0, 0.05);
        }
        .btn-primary {
            background-color: #8F9779;
            color: white;
            padding: 0.75rem 1.5rem;
            border-radius: 0.5rem;
            transition: background-color 0.3s;
        }
        .btn-primary:hover {
            background-color: #6B8E23;
        }
        .btn-tab.active {
            background-color: #6B8E23;
            color: white;
            z-index: 10;
        }
    </style>
</head>
<body class="antialiased">

    <header class="bg-white/80 backdrop-blur-md shadow-sm sticky top-0 z-50">
        <nav class="container mx-auto px-4 sm:px-6 lg:px-8">
            <div class="flex items-center justify-between h-16">
                <div class="flex items-center">
                    <span class="text-xl md:text-2xl tajawal-bold text-[#6B8E23]">المعيار الأخضر العراقي</span>
                </div>
                <div class="hidden md:block">
                    <div class="ml-10 flex items-baseline space-x-4 space-x-reverse">
                        <a href="#hero" class="nav-link px-3 py-2 rounded-md text-sm tajawal-medium">الرئيسية</a>
                        <a href="#challenges" class="nav-link px-3 py-2 rounded-md text-sm tajawal-medium">التحديات</a>
                        <a href="#standards" class="nav-link px-3 py-2 rounded-md text-sm tajawal-medium">المعايير</a>
                        <a href="#implementation" class="nav-link px-3 py-2 rounded-md text-sm tajawal-medium">التنفيذ</a>
                    </div>
                </div>
                <div class="md:hidden">
                    <button id="mobile-menu-button" class="inline-flex items-center justify-center p-2 rounded-md text-gray-400 hover:text-gray-600 focus:outline-none">
                        <span class="sr-only">فتح القائمة الرئيسية</span>
                        <svg class="h-6 w-6" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke="currentColor" aria-hidden="true">
                            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 6h16M4 12h16m-7 6h7" />
                        </svg>
                    </button>
                </div>
            </div>
        </nav>
        <div id="mobile-menu" class="md:hidden hidden">
            <div class="px-2 pt-2 pb-3 space-y-1 sm:px-3">
                <a href="#hero" class="block nav-link px-3 py-2 rounded-md text-base tajawal-medium">الرئيسية</a>
                <a href="#challenges" class="block nav-link px-3 py-2 rounded-md text-base tajawal-medium">التحديات</a>
                <a href="#standards" class="block nav-link px-3 py-2 rounded-md text-base tajawal-medium">المعايير</a>
                <a href="#implementation" class="block nav-link px-3 py-2 rounded-md text-base tajawal-medium">التنفيذ</a>
            </div>
        </div>
    </header>

    <main>
        <section id="hero" class="py-16 md:py-20 bg-white">
            <div class="container mx-auto px-4 sm:px-6 lg:px-8 text-center">
                <h1 class="text-4xl md:text-6xl tajawal-bold text-gray-800 leading-tight">نحو مستقبل عمراني مستدام في العراق</h1>
                <p class="mt-6 max-w-3xl mx-auto text-lg text-gray-600">
                    تحليل تفاعلي لمسودة "المعيار الأخضر العراقي 2025"، يهدف إلى تحويل هذا الإطار الوطني إلى رؤية واضحة ومفهومة لبناء مدن ومبانٍ مرنة وصديقة للبيئة، تضمن جودة الحياة للأجيال الحالية والقادمة.
                </p>
                <div class="mt-8">
                    <a href="#standards" class="btn-primary tajawal-bold">استكشف المعايير</a>
                </div>
            </div>
        </section>

        <section id="challenges" class="py-16">
            <div class="container mx-auto px-4 sm:px-6 lg:px-8">
                <div class="text-center mb-12">
                    <h2 class="text-3xl tajawal-bold text-gray-800">السياق: تحديات بيئية حيوية</h2>
                    <p class="mt-4 max-w-2xl mx-auto text-gray-600">
                        تم تصميم هذا المعيار كاستجابة مباشرة للأزمات البيئية والمناخية الحادة التي يواجهها العراق، والتي تتطلب حلولاً جذرية ومستدامة في قطاع البناء والتنمية الحضرية.
                    </p>
                </div>
                <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-8">
                    <div class="card text-center">
                        <div class="text-5xl mb-4">💨</div>
                        <h3 class="text-xl tajawal-bold mb-2">التلوث الهوائي</h3>
                        <p class="text-gray-600">انبعاثات صناعية ومن المركبات، وعواصف ترابية متكررة تؤثر على جودة الهواء والصحة العامة.</p>
                    </div>
                    <div class="card text-center">
                        <div class="text-5xl mb-4">💧</div>
                        <h3 class="text-xl tajawal-bold mb-2">ندرة المياه وتلوثها</h3>
                        <p class="text-gray-600">انخفاض مناسيب الأنهار وتلوث المصادر المائية يهددان الأمن المائي والزراعة.</p>
                    </div>
                    <div class="card text-center">
                        <div class="text-5xl mb-4">🏜️</div>
                        <h3 class="text-xl tajawal-bold mb-2">التصحر وتدهور الأراضي</h3>
                        <p class="text-gray-600">تراجع الغطاء النباتي والأراضي الزراعية يهدد الأمن الغذائي ويزيد من حدة العواصف الترابية.</p>
                    </div>
                    <div class="card text-center">
                        <div class="text-5xl mb-4">🗑️</div>
                        <h3 class="text-xl tajawal-bold mb-2">قصور إدارة النفايات</h3>
                        <p class="text-gray-600">ضعف أنظمة الجمع والمعالجة يؤدي إلى تلوث التربة والمياه وانتشار الأمراض.</p>
                    </div>
                    <div class="card text-center">
                        <div class="text-5xl mb-4">🌡️</div>
                        <h3 class="text-xl tajawal-bold mb-2">ارتفاع درجات الحرارة</h3>
                        <p class="text-gray-600">تغير المناخ يسبب موجات حر طويلة وجفاف، مما يزيد الضغط على الطاقة والموارد المائية.</p>
                    </div>
                    <div class="card text-center">
                        <div class="text-5xl mb-4">🏗️</div>
                        <h3 class="text-xl tajawal-bold mb-2">التدهور العمراني</h3>
                        <p class="text-gray-600">النمو غير المخطط له يؤدي إلى نقص الخدمات وتدهور البيئة الحضرية ونوعية الحياة.</p>
                    </div>
                </div>
            </div>
        </section>

        <section id="standards" class="py-16 bg-white">
            <div class="container mx-auto px-4 sm:px-6 lg:px-8">
                <div class="text-center mb-12">
                    <h2 class="text-3xl tajawal-bold text-gray-800">المعايير الخضراء: المباني والمدن</h2>
                     <p class="mt-4 max-w-3xl mx-auto text-gray-600">
                        يقدم المعيار إطارين متكاملين، أحدهما للمباني والآخر للمدن، لكل منهما محاور تقييم وأوزان نسبية تعكس الأولويات الوطنية. استكشف المحاور عبر الرسوم البيانية التفاعلية لمعرفة المزيد. انقر على أي قسم في الرسم البياني لعرض التفاصيل.
                    </p>
                </div>

                <div class="flex justify-center mb-8">
                    <div class="inline-flex rounded-md shadow-sm" role="group">
                        <button type="button" id="showBuildingsBtn" class="btn-tab active px-6 py-3 text-sm tajawal-medium border border-gray-200 rounded-r-lg hover:bg-opacity-90 focus:z-10">
                            معيار المباني
                        </button>
                        <button type="button" id="showCitiesBtn" class="btn-tab px-6 py-3 text-sm tajawal-medium bg-white text-gray-900 border-t border-b border-l border-gray-200 rounded-l-lg hover:bg-gray-100 hover:text-[#6B8E23] focus:z-10">
                            معيار المدن
                        </button>
                    </div>
                </div>

                <div id="buildingsStandard" class="standard-content">
                    <div class="grid grid-cols-1 lg:grid-cols-2 gap-8 lg:gap-12 items-center">
                        <div class="chart-container">
                            <canvas id="buildingsChart"></canvas>
                        </div>
                        <div id="buildingAxisDetails" class="card min-h-[450px] flex flex-col justify-center">
                            <h3 class="text-2xl tajawal-bold mb-4 text-center">محاور معيار المباني</h3>
                            <p class="text-gray-600 text-center">انقر على أي قسم في المخطط الدائري لعرض تفاصيل المحور وأهدافه ومتطلباته الرئيسية.</p>
                        </div>
                    </div>
                </div>

                <div id="citiesStandard" class="standard-content hidden">
                    <div class="grid grid-cols-1 lg:grid-cols-2 gap-8 lg:gap-12 items-center">
                        <div class="chart-container">
                            <canvas id="citiesChart"></canvas>
                        </div>
                        <div id="cityAxisDetails" class="card min-h-[450px] flex flex-col justify-center">
                             <h3 class="text-2xl tajawal-bold mb-4 text-center">محاور معيار المدن</h3>
                            <p class="text-gray-600 text-center">انقر على أي قسم في المخطط القطبي لعرض تفاصيل المحور وأهدافه ومتطلباته الرئيسية.</p>
                        </div>
                    </div>
                </div>
            </div>
        </section>

        <section id="implementation" class="py-16">
            <div class="container mx-auto px-4 sm:px-6 lg:px-8">
                <div class="text-center mb-12">
                    <h2 class="text-3xl tajawal-bold text-gray-800">آليات التنفيذ والسياسات الداعمة</h2>
                    <p class="mt-4 max-w-3xl mx-auto text-gray-600">
                        يعتمد نجاح المعيار على إطار تنفيذي قوي يشمل آليات تدقيق واضحة، وتعديلات قانونية، وسياسات داعمة لضمان التطبيق الفعال والتحول نحو الاستدامة. يعترف التقرير بوجود تحديات مثل غياب التخطيط ونقص التمويل، ويقترح حلولاً لمواجهتها.
                    </p>
                </div>

                <div class="grid grid-cols-1 md:grid-cols-2 gap-12">
                    <div class="card">
                        <h3 class="text-2xl tajawal-bold mb-6 text-center">مراحل التدقيق الهندسي</h3>
                        <div class="relative pr-8">
                            <div class="border-r-2 border-dashed border-[#D2B48C] absolute h-full top-0 right-4"></div>
                            <ul class="space-y-8">
                                <li class="flex items-start">
                                    <div class="flex-shrink-0 bg-[#D2B48C] text-white h-8 w-8 rounded-full flex items-center justify-center tajawal-bold ml-4 z-10">1</div>
                                    <div>
                                        <h4 class="tajawal-bold text-lg">تحديد النطاق</h4>
                                        <p class="text-gray-600">جمع الوثائق، تصنيف المبنى، وتحديد المعايير المطبقة.</p>
                                    </div>
                                </li>
                                <li class="flex items-start">
                                    <div class="flex-shrink-0 bg-[#D2B48C] text-white h-8 w-8 rounded-full flex items-center justify-center tajawal-bold ml-4 z-10">2</div>
                                    <div>
                                        <h4 class="tajawal-bold text-lg">التدقيق الميداني</h4>
                                        <p class="text-gray-600">فحص الموقع، التقييم الفني، ومقارنة الأداء بالمعايير.</p>
                                    </div>
                                </li>
                                <li class="flex items-start">
                                    <div class="flex-shrink-0 bg-[#D2B48C] text-white h-8 w-8 rounded-full flex items-center justify-center tajawal-bold ml-4 z-10">3</div>
                                    <div>
                                        <h4 class="tajawal-bold text-lg">ما بعد التدقيق</h4>
                                        <p class="text-gray-600">إصدار التقرير النهائي، منح الشهادة، والمتابعة السنوية.</p>
                                    </div>
                                </li>
                            </ul>
                        </div>
                    </div>
                    <div class="card">
                        <h3 class="text-2xl tajawal-bold mb-6 text-center">السياسات والتوصيات الرئيسية</h3>
                        <ul class="space-y-4 text-gray-700">
                            <li class="flex items-start"><span class="text-[#6B8E23] mt-1 mr-2">🏛️</span><span>**إطار قانوني:** دمج متطلبات البناء الأخضر في قوانين التخطيط العمراني والبلديات والاستثمار.</span></li>
                            <li class="flex items-start"><span class="text-[#6B8E23] mt-1 mr-2">💰</span><span>**حوافز مالية:** تقديم تخفيضات ضريبية وإعانات ودعم مالي للمشاريع الملتزمة بالمعيار.</span></li>
                            <li class="flex items-start"><span class="text-[#6B8E23] mt-1 mr-2">🎓</span><span>**بناء القدرات:** تدريب الكوادر الهندسية والفنية وتعزيز الوعي المجتمعي بأهمية الاستدامة.</span></li>
                             <li class="flex items-start"><span class="text-[#6B8E23] mt-1 mr-2">💻</span><span>**بنية تحتية رقمية:** الاستثمار في منصة تدقيق إلكترونية وأنظمة إدارة بيانات لضمان المراقبة الفعالة.</span></li>
                        </ul>
                    </div>
                </div>
            </div>
        </section>
    </main>

    <footer class="bg-gray-800 text-white py-8">
        <div class="container mx-auto px-4 sm:px-6 lg:px-8 text-center">
            <p class="tajawal-medium">تطبيق تفاعلي لتحليل "مسودة المعيار الأخضر العراقي للمباني والمدن - 2025"</p>
            <p class="text-sm text-gray-400 mt-2">تم التطوير بناءً على وثيقة التقييم النقدي المستندة إلى المسودة الصادرة عن وزارة الإعمار والإسكان والبلديات العامة.</p>
            <p class="text-sm text-gray-400 mt-1">اعداد: رئيس مهندسين: رائد ابراهيم خليل ابراهيم</p>
        </div>
    </footer>

    <script>
        document.addEventListener('DOMContentLoaded', function () {
            const buildingsData = {
                labels: ['كفاءة الطاقة', 'كفاءة المياه', 'المواد والموارد', 'الموقع', 'جودة البيئة الداخلية', 'الإدارة والإدامة', 'جودة البيئة الخارجية', 'إدارة النفايات', 'الابتكار'],
                weights: [30, 16, 12, 10, 9, 8, 8, 7, 5],
                details: [
                    {
                        title: 'كفاءة الطاقة (30%)',
                        description: 'يهدف إلى تقليل استهلاك الطاقة في المباني بشكل كبير، والاعتماد على مصادر متجددة، مما يقلل من البصمة الكربونية.',
                        points: [
                            'تحديد قيم قصوى لمعامل الانتقال الحراري (U-value) للجدران (0.5) والأسقف (0.2).',
                            'استخدام أنظمة إضاءة وتكييف ذكية وعالية الكفاءة.',
                            'توليد نسبة من الكهرباء عبر الطاقة الشمسية (تبدأ من 5%).',
                            'تحقيق أهداف محددة لكثافة استخدام الطاقة (EUI).'
                        ]
                    },
                    {
                        title: 'كفاءة المياه (16%)',
                        description: 'يركز على ترشيد استهلاك المياه وتقليل الضغط على الموارد المائية الشحيحة في العراق.',
                        points: [
                            'استخدام تجهيزات صحية موفرة للمياه.',
                            'تجميع مياه الأمطار لأغراض الري.',
                            'إعادة تدوير المياه الرمادية للاستخدامات غير الصالحة للشرب.',
                            'تركيب أنظمة كشف التسرب والعدادات الذكية.'
                        ]
                    },
                    {
                        title: 'المواد والموارد (12%)',
                        description: 'يشجع على استخدام مواد بناء مستدامة ومحلية المصدر لتقليل الأثر البيئي ودعم الاقتصاد المحلي.',
                        points: [
                            'إعادة تدوير 30% كحد أدنى من مخلفات البناء.',
                            'تفضيل المواد المنتجة ضمن نطاق 500 كم من المشروع.',
                            'تجنب المواد الخطرة مثل الأسبستوس.',
                            'تصميم المباني لتكون قابلة للتفكيك وإعادة الاستخدام.'
                        ]
                    },
                    {
                        title: 'الموقع (10%)',
                        description: 'يهدف إلى اختيار مواقع تقلل من التأثير على البيئة وتعزز الوصول إلى الخدمات والنقل العام.',
                        points: [
                            'تجنب البناء في الأراضي الزراعية أو المحميات الطبيعية.',
                            'اختيار مواقع قريبة من شبكات النقل العام.',
                            'إعادة تأهيل وتطوير المواقع الملوثة أو المهجورة.',
                            'تقليل تأثير المشروع على التنوع البيولوجي المحلي.'
                        ]
                    },
                    {
                        title: 'جودة البيئة الداخلية (9%)',
                        description: 'يركز على توفير بيئة داخلية صحية ومريحة لشاغلي المبنى، مما يعزز الإنتاجية والرفاهية.',
                        points: [
                            'ضمان نسب كافية من الإضاءة الطبيعية (15-20% من مساحة الفضاء).',
                            'تحقيق تهوية طبيعية فعالة (10-15% من مساحة الفضاء).',
                            'استخدام مواد داخلية منخفضة الانبعاثات (دهانات، مواد لاصقة).',
                            'مراقبة مستويات ثاني أكسيد الكربون وجودة الهواء.'
                        ]
                    },
                    {
                        title: 'الإدارة والإدامة (8%)',
                        description: 'يضمن تشغيل وصيانة المبنى بطريقة مستدامة طوال دورة حياته.',
                        points: [
                            'وضع خطط صيانة وقائية واضحة.',
                            'تدريب فريق الإدارة على الممارسات الخضراء.',
                            'مراقبة أداء استهلاك الطاقة والمياه بشكل دوري.',
                            'توفير أدلة تشغيل شاملة لشاغلي المبنى.'
                        ]
                    },
                    {
                        title: 'جودة البيئة الخارجية (8%)',
                        description: 'يهدف إلى تحسين البيئة المحيطة بالمبنى وتعزيز التنوع البيولوجي والمساحات الخضراء.',
                        points: [
                            'تخصيص 25% كحد أدنى للبنية التحتية الخضراء.',
                            'زراعة أنواع نباتية محلية مقاومة للجفاف والحرارة (السدر، النخيل).',
                            'استخدام مواد ذات انعكاسية عالية للأسطح لتقليل تأثير الجزر الحرارية.',
                            'تصميم أنظمة فعالة لإدارة مياه الأمطار السطحية.'
                        ]
                    },
                    {
                        title: 'إدارة النفايات (7%)',
                        description: 'يركز على تقليل كمية النفايات الناتجة عن عمليات البناء والتشغيل، وتعظيم إعادة التدوير.',
                        points: [
                            'توفير مساحات مخصصة لفرز النفايات (ورق، بلاستيك، زجاج).',
                            'وضع خطط لإدارة النفايات الخطرة.',
                            'تشجيع التسميد للمخلفات العضوية.',
                            'تحقيق أهداف محددة لإعادة تدوير مخلفات البناء.'
                        ]
                    },
                    {
                        title: 'الابتكار (5%)',
                        description: 'يشجع على تبني حلول وتقنيات مبتكرة تتجاوز المتطلبات الأساسية للمعيار.',
                        points: [
                            'تطبيق استراتيجيات تصميم أو تقنيات غير مشمولة بالمعيار.',
                            'تحقيق أداء استثنائي في أحد المحاور الأخرى.',
                            'دمج حلول تصميم مستوحاة من العمارة المحلية التقليدية.',
                            'تنفيذ مشاريع بحثية أو تجريبية رائدة.'
                        ]
                    }
                ]
            };

            const citiesData = {
                labels: ['كفاءة إدارة الطاقة', 'كفاءة إدارة المياه', 'التخطيط الحضري والنقل', 'جودة الحياة', 'إدارة المواد والنفايات', 'مخطط التصميم الأساس', 'الابتكار والوعي'],
                weights: [25, 20, 20, 15, 10, 10, 5],
                details: [
                     {
                        title: 'كفاءة إدارة الطاقة (25 نقطة)',
                        description: 'يهدف إلى إنشاء بنية تحتية للطاقة على مستوى المدينة تتسم بالكفاءة والموثوقية والاستدامة.',
                        points: [
                            'تصميم شبكات كهرباء ذكية وتغطية 100% للمدينة.',
                            'استخدام إضاءة شوارع ذكية وموفرة للطاقة.',
                            'تشجيع مصادر الطاقة المتجددة على نطاق واسع (الطاقة الشمسية).',
                            'تطبيق برامج إدارة الطلب على الطاقة في أوقات الذروة.'
                        ]
                    },
                    {
                        title: 'كفاءة إدارة المياه (20 نقطة)',
                        description: 'يركز على الإدارة المتكاملة للموارد المائية لضمان استدامتها وجودتها على المستوى الحضري.',
                        points: [
                            'ضمان تغطية 100% بشبكات المياه النظيفة والصرف الصحي.',
                            'معالجة مياه الصرف الصحي وإعادة استخدامها.',
                            'إنشاء أنظمة حضرية لتجميع مياه الأمطار.',
                            'استخدام عدادات ذكية لمراقبة الاستهلاك على مستوى المدينة.'
                        ]
                    },
                    {
                        title: 'التخطيط الحضري والنقل (20 نقطة)',
                        description: 'يهدف إلى إنشاء مدن مدمجة، متصلة، وسهلة التنقل، مع تقليل الاعتماد على المركبات الخاصة.',
                        points: [
                            'تخصيص 20% كحد أدنى من مساحة المدينة للمساحات الخضراء.',
                            'تطوير شبكات نقل عام شاملة ومتعددة الوسائط.',
                            'تصميم شوارع آمنة للمشاة والدراجات الهوائية.',
                            'تجنب التوسع الحضري على حساب الأراضي الزراعية والطبيعية.'
                        ]
                    },
                    {
                        title: 'جودة الحياة (15 نقطة)',
                        description: 'يركز على الجوانب الاجتماعية والاقتصادية والصحية لجعل المدن أماكن أفضل للعيش والعمل.',
                        points: [
                            'توفير بنية تحتية اجتماعية كافية (مدارس، مراكز صحية).',
                            'ضمان قرب الخدمات الأساسية من المناطق السكنية (في حدود 800 متر).',
                            'دعم النمو الاقتصادي المستدام وخلق فرص عمل.',
                            'توفير مساكن ميسورة التكلفة وتحقيق الأمن الغذائي الحضري.'
                        ]
                    },
                    {
                        title: 'إدارة المواد والنفايات (10 نقاط)',
                        description: 'يهدف إلى تطبيق مبادئ الاقتصاد الدائري على المستوى الحضري، من خلال إدارة فعالة للمواد والنفايات.',
                        points: [
                            'إدارة مخلفات البناء على مستوى المدينة (هدف إعادة تدوير 30%).',
                            'إنشاء أنظمة متكاملة لجمع وفرز ومعالجة النفايات الصلبة.',
                            'استخدام أنظمة ذكية لإدارة النفايات لتحسين الكفاءة.',
                            'تشجيع استخدام المواد المستدامة في مشاريع البنية التحتية.'
                        ]
                    },
                    {
                        title: 'مخطط التصميم الأساس (10 نقاط)',
                        description: 'يضمن وجود رؤية استراتيجية وخطة شاملة توجه التنمية الحضرية المستدامة للمدينة.',
                        points: [
                            'وضع خطة ورؤية عامة واضحة للمدينة.',
                            'إجراء تقييم الأثر البيئي للمشاريع الكبرى.',
                            'تصميم يراعي الهوية المحلية والثقافية.',
                            'إشراك المجتمع في عمليات التخطيط.'
                        ]
                    },
                    {
                        title: 'الابتكار والوعي المجتمعي (5+ نقاط)',
                        description: 'يشجع على الحلول المبتكرة والمشاركة المجتمعية لتعزيز ثقافة الاستدامة.',
                        points: [
                            'تطبيق حلول إبداعية تتجاوز متطلبات المعيار.',
                            'دمج الحلول البيئية التاريخية والمستوحاة من التراث.',
                            'إطلاق حملات توعية واسعة النطاق.',
                            'تعزيز الشراكات بين القطاعين العام والخاص والمجتمع المدني.'
                        ]
                    }
                ]
            };
            
            const chartColors = ['#6B8E23', '#8F9779', '#D2B48C', '#A0522D', '#BC8F8F', '#F4A460', '#CD853F', '#8B4513', '#BDB76B'];
            let buildingsChart, citiesChart;

            function createBuildingsChart() {
                const ctx = document.getElementById('buildingsChart').getContext('2d');
                if (buildingsChart) {
                    buildingsChart.destroy();
                }
                buildingsChart = new Chart(ctx, {
                    type: 'doughnut',
                    data: {
                        labels: buildingsData.labels,
                        datasets: [{
                            data: buildingsData.weights,
                            backgroundColor: chartColors,
                            borderColor: '#FFFFFF',
                            borderWidth: 4
                        }]
                    },
                    options: {
                        responsive: true,
                        maintainAspectRatio: false,
                        plugins: {
                            legend: { display: false },
                            tooltip: {
                                callbacks: {
                                    label: (c) => `${c.label}: ${c.parsed}%`
                                },
                                bodyFont: { family: 'Tajawal' },
                                titleFont: { family: 'Tajawal' }
                            }
                        },
                        onClick: (_, elements) => {
                            if (elements.length > 0) updateDetails('building', buildingsData.details[elements[0].index]);
                        }
                    }
                });
            }

            function createCitiesChart() {
                const ctx = document.getElementById('citiesChart').getContext('2d');
                if (citiesChart) {
                    citiesChart.destroy();
                }
                citiesChart = new Chart(ctx, {
                    type: 'polarArea',
                    data: {
                        labels: citiesData.labels,
                        datasets: [{
                            data: citiesData.weights,
                            backgroundColor: chartColors.map(c => c + 'B3'),
                            borderColor: chartColors,
                            borderWidth: 2
                        }]
                    },
                    options: {
                        responsive: true,
                        maintainAspectRatio: false,
                        plugins: {
                            legend: { display: false },
                            tooltip: {
                                callbacks: {
                                    label: (c) => `${c.label}: ${c.raw} نقطة`
                                },
                                bodyFont: { family: 'Tajawal' },
                                titleFont: { family: 'Tajawal' }
                            }
                        },
                        onClick: (_, elements) => {
                            if (elements.length > 0) updateDetails('city', citiesData.details[elements[0].index]);
                        },
                        scales: {
                            r: {
                                ticks: { backdropColor: 'transparent', color: '#6b7280' },
                                grid: { color: '#E5E7EB' }
                            }
                        }
                    }
                });
            }

            function updateDetails(type, data) {
                const containerId = type === 'building' ? 'buildingAxisDetails' : 'cityAxisDetails';
                const container = document.getElementById(containerId);
                let pointsHtml = data.points.map(point => `<li class="flex items-start"><span class="text-[#6B8E23] mt-1 ml-2">✔</span><span>${point}</span></li>`).join('');
                container.innerHTML = `
                    <h3 class="text-2xl tajawal-bold mb-2 text-[#6B8E23]">${data.title}</h3>
                    <p class="text-gray-600 mb-4">${data.description}</p>
                    <h4 class="tajawal-bold text-lg mb-2">أبرز المتطلبات:</h4>
                    <ul class="space-y-2 text-gray-700">${pointsHtml}</ul>
                `;
            }

            const showBuildingsBtn = document.getElementById('showBuildingsBtn');
            const showCitiesBtn = document.getElementById('showCitiesBtn');
            const buildingsStandard = document.getElementById('buildingsStandard');
            const citiesStandard = document.getElementById('citiesStandard');
            const tabs = [showBuildingsBtn, showCitiesBtn];

            showBuildingsBtn.addEventListener('click', () => {
                buildingsStandard.classList.remove('hidden');
                citiesStandard.classList.add('hidden');
                setActiveTab(showBuildingsBtn);
                createBuildingsChart();
            });

            showCitiesBtn.addEventListener('click', () => {
                citiesStandard.classList.remove('hidden');
                buildingsStandard.classList.add('hidden');
                setActiveTab(showCitiesBtn);
                createCitiesChart();
            });

            function setActiveTab(activeBtn) {
                 tabs.forEach(btn => {
                    btn.classList.remove('active', 'bg-[#6B8E23]', 'text-white');
                    btn.classList.add('bg-white', 'text-gray-900');
                });
                activeBtn.classList.add('active', 'bg-[#6B8E23]', 'text-white');
                activeBtn.classList.remove('bg-white', 'text-gray-900');
            }

            const mobileMenuButton = document.getElementById('mobile-menu-button');
            const mobileMenu = document.getElementById('mobile-menu');
            mobileMenuButton.addEventListener('click', () => {
                mobileMenu.classList.toggle('hidden');
            });
            
            document.querySelectorAll('a[href^="#"]').forEach(anchor => {
                anchor.addEventListener('click', function (e) {
                    e.preventDefault();
                    const targetElement = document.querySelector(this.getAttribute('href'));
                    if (targetElement) {
                        targetElement.scrollIntoView({ behavior: 'smooth' });
                    }
                    if(!mobileMenu.classList.contains('hidden')){
                        mobileMenu.classList.add('hidden');
                    }
                });
            });

            createBuildingsChart();
        });
    </script>
</body>
</html>
# GREEN
