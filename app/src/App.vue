<script setup>
import { computed, reactive, ref, watch } from 'vue'

const steps = [
  {
    id: 'project',
    title: 'Definición del proyecto',
    subtitle: 'Contexto y alcance inicial',
  },
  {
    id: 'discovery',
    title: 'Preguntas de afinado',
    subtitle: 'Ajusta supuestos clave antes del mapeo',
  },
  {
    id: 'stakeholders',
    title: 'Catálogo de actores',
    subtitle: 'Intereses, influencia y actitudes',
  },
  {
    id: 'map',
    title: 'Mapa visual',
    subtitle: 'Prioriza y prepara la estrategia',
  },
]

const currentStep = ref(0)

const project = reactive({
  sponsor: '',
  description: '',
  location: '',
  phase: '',
  mainObjective: '',
  sensitiveTopics: '',
  knownActors: '',
  milestones: [],
  references: [],
})

const stakeholders = ref([])

function getStakeholderList() {
  if (!Array.isArray(stakeholders.value)) {
    stakeholders.value = []
  }

  return stakeholders.value
}

const stakeholderList = computed(() => getStakeholderList())
const stakeholderCount = computed(() => stakeholderList.value.length)

const stakeholderSeed = [
  {
    name: 'Acciona Agua, S.A.',
    type: 'Entidad',
    category: 'Empresa promotora',
    role: 'EPC/O&M del proyecto; relación institucional y técnica',
    organization: 'Acciona',
  },
  {
    name: 'Rafael Mateo',
    type: 'Persona',
    category: 'Empresa (energía)',
    role: 'CEO Acciona Energía; potencial PPA renovable',
    organization: 'Acciona Energía',
  },
  {
    name: 'José Manuel Entrecanales',
    type: 'Persona',
    category: 'Empresa (grupo)',
    role: 'Presidente Acciona; gobernanza corporativa y reputación',
    organization: 'Acciona, S.A.',
  },
  {
    name: 'Teresa Ribera',
    type: 'Persona',
    category: 'Gobierno central',
    role: 'Vicepresidenta y Ministra; tutela ambiental y energética',
    organization: 'MITECO',
  },
  {
    name: 'Dirección Gral. de la Costa y el Mar',
    type: 'Entidad',
    category: 'Gobierno central',
    role: 'Autorización de ocupación DPMT; emisario y toma',
    organization: 'MITECO',
  },
  {
    name: 'Demarcación de Costas Andalucía-Oriental',
    type: 'Entidad',
    category: 'Gobierno central',
    role: 'Informes y control de proyecto litoral',
    organization: 'MITECO',
  },
  {
    name: 'Acuamed (Aguas de las Cuencas Mediterráneas)',
    type: 'Entidad',
    category: 'Entidad estatal',
    role: 'Promoción/gestión de infraestructuras de desalación',
    organization: 'MITECO',
  },
  {
    name: 'Red Eléctrica (Redeia)',
    type: 'Entidad',
    category: 'Infraestructura',
    role: 'Conexión y capacidad de red/subestación',
    organization: 'Sistema eléctrico',
  },
  {
    name: 'Instituto para la Diversificación y Ahorro de la Energía (IDAE)',
    type: 'Entidad',
    category: 'Agencia estatal',
    role: 'Apoyo a eficiencia/renovables; posibles ayudas',
    organization: 'MITECO',
  },
  {
    name: 'Juanma Moreno',
    type: 'Persona',
    category: 'Gobierno autonómico',
    role: 'Presidente Junta; agenda política regional',
    organization: 'Junta de Andalucía',
  },
  {
    name: 'Carmen Crespo',
    type: 'Persona',
    category: 'Gobierno autonómico',
    role: 'Consejera de Agricultura, Pesca, Agua y Desarrollo Rural',
    organization: 'Junta de Andalucía',
  },
  {
    name: 'Consejería de Sostenibilidad, Medio Ambiente y Economía Azul',
    type: 'Entidad',
    category: 'Gobierno autonómico',
    role: 'Autorizaciones ambientales/espacios protegidos',
    organization: 'Junta de Andalucía',
  },
  {
    name: 'Dirección Gral. de Planificación y Recursos Hídricos',
    type: 'Entidad',
    category: 'Gobierno autonómico',
    role: 'Planificación hídrica autonómica (CMA)',
    organization: 'Junta de Andalucía',
  },
  {
    name: 'Alcaldía de Níjar',
    type: 'Entidad',
    category: 'Gobierno local',
    role: 'Licencias urbanísticas y coordinación local',
    organization: 'Ayuntamiento de Níjar',
  },
  {
    name: 'Javier A. García',
    type: 'Persona',
    category: 'Gobierno provincial',
    role: 'Presidente Diputación de Almería; coordinación territorial',
    organization: 'Diputación de Almería',
  },
  {
    name: 'Autoridad Portuaria de Almería',
    type: 'Entidad',
    category: 'Puerto',
    role: 'Servidumbres y coordinación marítima si aplica',
    organization: 'Puerto de Almería',
  },
  {
    name: 'Agencia Andaluza del Agua (CMA)',
    type: 'Entidad',
    category: 'Agua autonómica',
    role: 'Gestión de Cuencas Mediterráneas Andaluzas',
    organization: 'Junta de Andalucía',
  },
  {
    name: 'Consorcios de abastecimiento Levante Almeriense',
    type: 'Entidad',
    category: 'Operador agua',
    role: 'Contrato de suministro mayorista',
    organization: 'Andalucía',
  },
  {
    name: 'COEXPHAL',
    type: 'Entidad',
    category: 'Asociación empresarial',
    role: 'Asociación de productores-exportadores de Almería',
    organization: 'Agro-exportación',
  },
  {
    name: 'CASI (Coop. Agrícola San Isidro)',
    type: 'Entidad',
    category: 'Cooperativa agrícola',
    role: 'Consumo intensivo; contratos de offtake',
    organization: 'Almería',
  },
  {
    name: 'Unica Group',
    type: 'Entidad',
    category: 'Cooperativa agrícola',
    role: 'Consumo intensivo; exportación hortofrutícola',
    organization: 'Almería',
  },
  {
    name: 'Vicasol',
    type: 'Entidad',
    category: 'Cooperativa agrícola',
    role: 'Consumo intensivo; destino europeo',
    organization: 'Almería',
  },
  {
    name: 'Murgiverde',
    type: 'Entidad',
    category: 'Cooperativa agrícola',
    role: 'Consumo intensivo; transformación',
    organization: 'Almería',
  },
  {
    name: 'ASAJA Almería',
    type: 'Entidad',
    category: 'Sindicato agrario',
    role: 'Representación empresarial agraria',
    organization: 'Almería',
  },
  {
    name: 'COAG Almería',
    type: 'Entidad',
    category: 'Sindicato agrario',
    role: 'Representación agraria y precios del agua',
    organization: 'Almería',
  },
  {
    name: 'UPA Andalucía',
    type: 'Entidad',
    category: 'Sindicato agrario',
    role: 'Pequeños productores y regadíos',
    organization: 'Andalucía',
  },
  {
    name: 'Endesa Andalucía',
    type: 'Entidad',
    category: 'Empresa (energía)',
    role: 'Suministro y potencia contratada',
    organization: 'Andalucía',
  },
  {
    name: 'CSIC - Estación Experimental de Zonas Áridas (EEZA)',
    type: 'Entidad',
    category: 'Academia',
    role: 'Investigación ecosistemas áridos y biodiversidad',
    organization: 'CSIC Almería',
  },
  {
    name: 'Universidad de Almería (UAL)',
    type: 'Entidad',
    category: 'Academia',
    role: 'Peritaje hídrico y marino; formación dual',
    organization: 'Almería',
  },
  {
    name: 'Ecologistas en Acción Andalucía',
    type: 'Entidad',
    category: 'ONG ambiental',
    role: 'Evaluación crítica, litigio y campañas',
    organization: 'Tercer sector',
  },
  {
    name: 'Greenpeace España',
    type: 'Entidad',
    category: 'ONG ambiental',
    role: 'Campañas marinas y energía',
    organization: 'Tercer sector',
  },
  {
    name: 'SEO/BirdLife Andalucía',
    type: 'Entidad',
    category: 'ONG ambiental',
    role: 'Aves marinas y ZEPA costeras',
    organization: 'Tercer sector',
  },
  {
    name: 'Cofradía de Pescadores de Carboneras',
    type: 'Entidad',
    category: 'Sector pesquero',
    role: 'Interacciones con emisario y áreas de pesca',
    organization: 'Almería',
  },
  {
    name: 'Parque Natural Cabo de Gata-Níjar',
    type: 'Entidad',
    category: 'Espacio protegido',
    role: 'Compatibilidad ambiental y usos',
    organization: 'Junta de Andalucía',
  },
  {
    name: 'OCU',
    type: 'Entidad',
    category: 'Consumidores',
    role: 'Calidad y precio del agua',
    organization: 'España',
  },
  {
    name: 'FACUA',
    type: 'Entidad',
    category: 'Consumidores',
    role: 'Defensa del consumidor',
    organization: 'España',
  },
  {
    name: 'Asociaciones vecinales de Níjar',
    type: 'Entidad',
    category: 'Comunidad local',
    role: 'Licencia social y seguimiento',
    organization: 'Níjar',
  },
  {
    name: 'La Voz de Almería',
    type: 'Entidad',
    category: 'Medios',
    role: 'Cobertura regional',
    organization: 'Almería',
  },
  {
    name: 'Canal Sur',
    type: 'Entidad',
    category: 'Medios',
    role: 'Cobertura autonómica',
    organization: 'Andalucía',
  },
  {
    name: 'RTVE Andalucía',
    type: 'Entidad',
    category: 'Medios',
    role: 'Cobertura estatal/autonómica',
    organization: 'Andalucía',
  },
  {
    name: 'Divulgadores de clima/agua (Javier Peña, Andreu Escrivà)',
    type: 'Entidad',
    category: 'Medios digitales',
    role: 'Amplificación en RRSS',
    organization: 'España',
  },
  {
    name: 'Comisión Europea – DG ENV',
    type: 'Entidad',
    category: 'Institución UE',
    role: 'Directiva Hábitats/EIA; supervisión',
    organization: 'UE',
  },
  {
    name: 'Comisión Europea – DG MARE',
    type: 'Entidad',
    category: 'Institución UE',
    role: 'Política marítima y pesca',
    organization: 'UE',
  },
  {
    name: 'Banco Europeo de Inversiones (BEI)',
    type: 'Entidad',
    category: 'Finanzas UE',
    role: 'Financiación verde; taxonomía UE',
    organization: 'UE',
  },
  {
    name: 'Instituto de Crédito Oficial (ICO)',
    type: 'Entidad',
    category: 'Finanzas',
    role: 'Financiación nacional',
    organization: 'España',
  },
]

let milestoneId = 0
let referenceId = 0
let stakeholderId = 0

const discoveryTemplates = [
  {
    id: 'water-allocation',
    prompt: '¿Qué % aproximado del agua irá a agricultura vs. industria vs. municipal (si aplica)?',
    answer:
      '70% agricultura intensiva (invernaderos/transformación), 25% industria local (agroalimentaria/auxiliar), 5% municipal (puntas estacionales).',
  },
  {
    id: 'energy-mix',
    prompt:
      '¿Qué fuente energética tendrá la planta (mix actual y objetivo)? ¿Habrá PPA/renovables onsite (solar/eólica) para reducir huella?',
    answer:
      'Suministro de red con mix nacional; objetivo PPA renovable 80–100% y fotovoltaica onsite ~10–15% de cobertura anual (vertido a red + autoconsumo).',
  },
  {
    id: 'brine-solution',
    prompt:
      '¿Cuál es la solución prevista para la salmuera (emisario, difusores, dilución) y el plan de monitoreo ambiental?',
    answer:
      'Emisario submarino con difusores multipuerto, dilución ≥ 1:40 a 50–100 m; monitoreo trimestral (salinidad, temperatura, bentos, epibiontes) y estación continua en pluma.',
  },
  {
    id: 'intake-technology',
    prompt:
      '¿Qué tecnología de captación se prevé (toma abierta, pozos de playa, filtros) y qué medidas de protección de ictiofauna/hábitat marino?',
    answer:
      'Toma abierta con rejillas <5 mm, velocidad de aproximación <0,15 m/s y prefiltración; esclusas para mantenimiento y programa de exclusión acústica temporal.',
  },
  {
    id: 'water-pricing',
    prompt:
      '¿Tenéis identificados precios/umbrales de asequibilidad del agua para cooperativas y pymes locales?',
    answer:
      'Rango objetivo 0,55–0,70 €/m³ para contratos marco con cooperativas; cláusulas de indexación a energía con techo/bonos por eficiencia hídrica.',
  },
  {
    id: 'permits-status',
    prompt:
      '¿Qué permisos y administraciones clave están implicados (MITECO, Junta de Andalucía, Autoridad Portuaria/Costas, Ayuntamiento) y su estado?',
    answer:
      'EIA en curso (MITECO); informe de Costas/Demarcación Andalucía-Oriental; autorizaciones ambientales unificadas (Junta); licencia municipal condicionada al EIA.',
  },
  {
    id: 'agricultural-offtakers',
    prompt:
      '¿Qué actores agrícolas concretos consumen mayor volumen (cooperativas, SATs, exportadoras) y con qué contratos marco?',
    answer:
      'COEXPHAL (asociadas), CASI, Unica Group, Vicasol, Murgiverde; PPAs hídricos a 5–10 años con compromisos de eficiencia y calidad.',
  },
  {
    id: 'socioeconomic-impacts',
    prompt:
      '¿Qué impactos sociales/económicos se han valorado (empleo local, formación, proveedores) y compromisos medibles?',
    answer:
      'Empleo directo 150 obra/40 operación; 20% compras locales; plan de formación dual (UAL/FP) y programa de proveedores Km0.',
  },
  {
    id: 'risk-mitigation',
    prompt:
      '¿Qué riesgos veis como más probables (energía/coste, oposición ambiental, turismo/pesca, capacidad de red) y planes de mitigación?',
    answer:
      'Riesgo energía (PPA + FV onsite), oposición ambiental (restauración de praderas + transparencia), pesca/turismo (zonificación temporal), red (refuerzo subestación).',
  },
  {
    id: 'operational-schedule',
    prompt:
      '¿Qué calendario operativo manejáis (EIA 2025, permisos 2026, obra 2027) y qué dependencias críticas podrían adelantar/retrasar?',
    answer:
      'Fechas objetivo confirmadas; críticas: dictamen Costas, compatibilidad Red Eléctrica, servidumbres de emisario y acuerdos con cooperativas para offtake.',
  },
]

const discoveryQuestions = reactive(
  discoveryTemplates.map(({ id, prompt }) => ({
    id,
    prompt,
    answer: '',
  })),
)

const newStakeholder = reactive({
  name: '',
  type: '',
  category: '',
  role: '',
  organization: '',
  influence: 'medium',
  interest: 'medium',
  attitude: 'Neutral',
  notes: '',
})

const interestOptions = [
  { value: 'low', label: 'Bajo' },
  { value: 'medium', label: 'Medio' },
  { value: 'high', label: 'Alto' },
]

const attitudeOptions = [
  'Aliado',
  'Apoyo condicional',
  'Neutral',
  'Vigilante',
  'Preocupado',
  'Oposición',
]

const levelLabels = {
  low: 'Baja',
  medium: 'Media',
  high: 'Alta',
}

const influenceOrder = ['high', 'medium', 'low']
const interestOrder = ['low', 'medium', 'high']

const progressPercentage = computed(() => {
  if (steps.length === 1) {
    return 100
  }
  return Math.round((currentStep.value / (steps.length - 1)) * 100)
})

const canAdvance = computed(() => {
  if (currentStep.value === 0) {
    return (
      project.sponsor.trim() &&
      project.description.trim() &&
      project.location.trim() &&
      project.phase.trim() &&
      project.mainObjective.trim()
    )
  }

  if (currentStep.value === 1) {
    return discoveryQuestions.every((question) => question.answer.trim())
  }

  if (currentStep.value === 2) {
    return getStakeholderList().length > 0
  }

  return true
})

const progressBarStyle = computed(() => ({
  width: `${progressPercentage.value}%`,
}))

const matrix = computed(() => {
  const list = getStakeholderList()

  return influenceOrder.map((influence) => {
    return interestOrder.map((interest) => ({
      influence,
      interest,
      stakeholders: list.filter(
        (item) => item.influence === influence && item.interest === interest,
      ),
    }))
  })
})

function createMilestone(label = '', date = '') {
  return {
    id: ++milestoneId,
    label,
    date,
  }
}

function createReference(label = '', url = '') {
  return {
    id: ++referenceId,
    label,
    url,
  }
}

function createStakeholder(data = {}) {
  return {
    id: ++stakeholderId,
    name: data.name ?? '',
    type: data.type ?? '',
    segment: data.segment ?? data.category ?? '',
    category: data.category ?? data.segment ?? '',
    role: data.role ?? '',
    organization: data.organization ?? '',
    influence: data.influence ?? 'medium',
    interest: data.interest ?? 'medium',
    attitude: data.attitude ?? 'Neutral',
    notes: data.notes ?? '',
  }
}

function addMilestone() {
  project.milestones.push(createMilestone())
}

function removeMilestone(id) {
  const index = project.milestones.findIndex((item) => item.id === id)
  if (index >= 0) {
    project.milestones.splice(index, 1)
  }
}

function addReference() {
  project.references.push(createReference())
}

function removeReference(id) {
  const index = project.references.findIndex((item) => item.id === id)
  if (index >= 0) {
    project.references.splice(index, 1)
  }
}

function addStakeholder() {
  if (!newStakeholder.name.trim()) {
    return
  }

  const list = getStakeholderList()

  list.push(
    createStakeholder({
      ...newStakeholder,
    }),
  )

  newStakeholder.name = ''
  newStakeholder.type = ''
  newStakeholder.category = ''
  newStakeholder.role = ''
  newStakeholder.organization = ''
  newStakeholder.influence = 'medium'
  newStakeholder.interest = 'medium'
  newStakeholder.attitude = 'Neutral'
  newStakeholder.notes = ''
}

function removeStakeholder(id) {
  const list = getStakeholderList()

  const index = list.findIndex((item) => item.id === id)
  if (index >= 0) {
    list.splice(index, 1)
  }
}

function ensureStakeholderCatalog() {
  if (getStakeholderList().length === 0) {
    loadStakeholderSuggestions()
  }
}

function nextStep() {
  if (currentStep.value < steps.length - 1 && canAdvance.value) {
    currentStep.value += 1

    if (currentStep.value === 2) {
      ensureStakeholderCatalog()
    }
  }
}

function previousStep() {
  if (currentStep.value > 0) {
    currentStep.value -= 1
  }
}

function resetProject() {
  project.sponsor = ''
  project.description = ''
  project.location = ''
  project.phase = ''
  project.mainObjective = ''
  project.sensitiveTopics = ''
  project.knownActors = ''
  project.milestones.splice(0)
  project.references.splice(0)
}

function resetDiscovery() {
  discoveryQuestions.forEach((question) => {
    question.answer = ''
  })
}

function resetAll() {
  resetProject()
  resetDiscovery()
  stakeholders.value = []
  currentStep.value = 0
}

function applyStakeholderSeed(seed) {
  const normalizedSeed = Array.isArray(seed) ? seed : []

  stakeholders.value = normalizedSeed.map((item) =>
    createStakeholder({
      ...item,
    }),
  )
}

function loadStakeholderSuggestions() {
  applyStakeholderSeed(stakeholderSeed)
}

function autofillDiscovery() {
  discoveryQuestions.forEach((question) => {
    const template = discoveryTemplates.find((item) => item.id === question.id)
    if (template) {
      question.answer = template.answer
    }
  })
}

function autofill() {
  project.sponsor = 'Acciona Agua, S.A.'
  project.description =
    'Construcción de una nueva planta desaladora en el litoral de Almería, destinada a abastecer a la agricultura intensiva y a industrias locales, con capacidad de 80 hm³ anuales.'
  project.location = 'España · Andalucía · Níjar (Almería).'
  project.phase = 'Tramitación (en evaluación de impacto ambiental).'
  project.mainObjective = 'Económico / Estratégico.'
  project.sensitiveTopics = 'Agua · Energía · Agricultura intensiva · Impacto costero.'
  project.knownActors = [
    'Ministerio para la Transición Ecológica y Reto Demográfico.',
    'Junta de Andalucía (Consejería de Agricultura, Pesca, Agua y Desarrollo Rural).',
    'Ayuntamiento de Níjar.',
    'Sindicatos agrarios (COAG, ASAJA).',
    'Cooperativas agrícolas locales.',
    'Asociaciones ecologistas (Ecologistas en Acción, Greenpeace).',
    'Empresas exportadoras de frutas y hortalizas.',
    'Medios regionales (La Voz de Almería).',
  ].join('\n')

  project.milestones.splice(0, project.milestones.length, ...[
    createMilestone('Finalización del Estudio de Impacto Ambiental', '2025'),
    createMilestone('Resolución administrativa y permisos', '2026'),
    createMilestone('Inicio de construcción', '2027'),
  ])

  project.references.splice(0, project.references.length, ...[
    createReference('Acciona Agua - Proyectos de desalación', 'https://www.acciona.com/es/negocios/agua/desalacion/'),
  ])

  autofillDiscovery()

  applyStakeholderSeed(
    stakeholderSeed.map((item) => ({
      ...item,
      influence:
        ['MITECO', 'Junta de Andalucía', 'Acciona', 'UE'].some((keyword) =>
          [item.organization, item.category].some((field) =>
            field?.toLowerCase().includes(keyword.toLowerCase()),
          ),
        )
          ? 'high'
          : 'medium',
      interest: ['Cooperativa', 'Agrícola', 'Medios', 'Consumidores'].some((keyword) =>
        [item.category, item.role].some((field) => field?.toLowerCase().includes(keyword.toLowerCase())),
      )
        ? 'high'
        : 'medium',
      attitude:
        ['ONG', 'Ecologistas', 'SEO/BirdLife'].some((keyword) =>
          [item.category, item.name].some((field) => field?.toLowerCase().includes(keyword.toLowerCase())),
        )
          ? 'Vigilante'
          : 'Neutral',
    })),
  )
}

const nextButtonLabel = computed(() => (currentStep.value === 0 ? 'Iniciar proceso' : 'Continuar'))

watch(currentStep, (step) => {
  if (step === 2) {
    ensureStakeholderCatalog()
  }
})
</script>

<template>
  <div class="min-h-screen bg-slate-100">
    <div class="max-w-6xl mx-auto px-6 py-10">
      <div class="bg-white shadow-2xl rounded-3xl overflow-hidden">
        <div class="bg-gradient-to-r from-primary-700 via-primary-600 to-primary-500 px-8 py-10 text-white">
          <div class="flex flex-col gap-4 md:flex-row md:items-end md:justify-between">
            <div>
              <p class="text-sm uppercase tracking-[0.2em] text-white/70">Toolkit de participación</p>
              <h1 class="mt-2 text-3xl font-semibold md:text-4xl">
                Configurador de mapa de stakeholders
              </h1>
              <p class="mt-3 max-w-3xl text-white/80">
                Avanza paso a paso: define el proyecto, clasifica a los actores implicados y genera una visualización
                accionable para tu estrategia de relacionamiento.
              </p>
            </div>
            <div class="w-full max-w-sm">
              <div class="flex items-center justify-between text-xs font-medium uppercase text-white/70">
                <span>Paso {{ currentStep + 1 }} de {{ steps.length }}</span>
                <span>{{ progressPercentage }}%</span>
              </div>
              <div class="mt-2 h-2 rounded-full bg-white/20">
                <div class="h-2 rounded-full bg-white transition-all duration-300" :style="progressBarStyle" />
              </div>
              <div class="mt-4 grid gap-2 text-xs">
                <div
                  v-for="(step, index) in steps"
                  :key="step.id"
                  class="rounded-full border border-white/30 px-3 py-1"
                  :class="index === currentStep ? 'bg-white/90 text-primary-700 font-semibold' : 'text-white/80'"
                >
                  {{ step.title }}
                </div>
              </div>
            </div>
          </div>
        </div>

        <div class="p-8 md:p-10">
          <section v-if="currentStep === 0" class="space-y-8">
            <div class="flex flex-col gap-4 rounded-2xl border border-slate-200 bg-slate-50/60 p-6 md:flex-row md:items-center md:justify-between">
              <div>
                <h2 class="text-xl font-semibold text-slate-800">Paso 1 · Datos del proyecto</h2>
                <p class="mt-1 text-sm text-slate-600">
                  Completa la información clave para contextualizar el análisis. Puedes usar el botón de auto rellenar para
                  cargar un ejemplo de prueba.
                </p>
              </div>
              <button
                type="button"
                @click="autofill"
                class="inline-flex items-center justify-center gap-2 rounded-full bg-primary-600 px-5 py-2 text-sm font-semibold text-white shadow-lg shadow-primary-600/40 transition hover:bg-primary-500"
              >
                <span class="inline-block h-2 w-2 rounded-full bg-white" />
                Auto rellenar demo
              </button>
            </div>

            <form class="space-y-6">
              <div class="grid gap-6 md:grid-cols-2">
                <div class="space-y-2">
                  <label class="text-sm font-semibold text-slate-700">Empresa promotora</label>
                  <input
                    v-model="project.sponsor"
                    type="text"
                    class="w-full rounded-xl border border-slate-200 bg-white px-4 py-3 text-slate-800 shadow-inner shadow-slate-100 focus:border-primary-500 focus:outline-none focus:ring-2 focus:ring-primary-200"
                    placeholder="Nombre de la organización líder"
                  />
                </div>
                <div class="space-y-2">
                  <label class="text-sm font-semibold text-slate-700">Localización (país · región · municipio)</label>
                  <input
                    v-model="project.location"
                    type="text"
                    class="w-full rounded-xl border border-slate-200 bg-white px-4 py-3 text-slate-800 shadow-inner shadow-slate-100 focus:border-primary-500 focus:outline-none focus:ring-2 focus:ring-primary-200"
                    placeholder="Ej. España · Andalucía · Níjar"
                  />
                </div>
                <div class="space-y-2 md:col-span-2">
                  <label class="text-sm font-semibold text-slate-700">Descripción breve del proyecto</label>
                  <textarea
                    v-model="project.description"
                    rows="4"
                    class="w-full rounded-xl border border-slate-200 bg-white px-4 py-3 text-slate-800 shadow-inner shadow-slate-100 focus:border-primary-500 focus:outline-none focus:ring-2 focus:ring-primary-200"
                    placeholder="Resume el propósito y el alcance"
                  />
                </div>
                <div class="space-y-2">
                  <label class="text-sm font-semibold text-slate-700">Fase actual</label>
                  <input
                    v-model="project.phase"
                    type="text"
                    class="w-full rounded-xl border border-slate-200 bg-white px-4 py-3 text-slate-800 shadow-inner shadow-slate-100 focus:border-primary-500 focus:outline-none focus:ring-2 focus:ring-primary-200"
                    placeholder="Planeación, construcción, operación..."
                  />
                </div>
                <div class="space-y-2">
                  <label class="text-sm font-semibold text-slate-700">Objetivo principal</label>
                  <input
                    v-model="project.mainObjective"
                    type="text"
                    class="w-full rounded-xl border border-slate-200 bg-white px-4 py-3 text-slate-800 shadow-inner shadow-slate-100 focus:border-primary-500 focus:outline-none focus:ring-2 focus:ring-primary-200"
                    placeholder="Ej. Económico, social, ambiental"
                  />
                </div>
                <div class="space-y-2">
                  <label class="text-sm font-semibold text-slate-700">Temas sensibles percibidos</label>
                  <textarea
                    v-model="project.sensitiveTopics"
                    rows="3"
                    class="w-full rounded-xl border border-slate-200 bg-white px-4 py-3 text-slate-800 shadow-inner shadow-slate-100 focus:border-primary-500 focus:outline-none focus:ring-2 focus:ring-primary-200"
                    placeholder="Impactos, preocupaciones y focos de tensión"
                  />
                </div>
                <div class="space-y-2">
                  <label class="text-sm font-semibold text-slate-700">Actores iniciales conocidos</label>
                  <textarea
                    v-model="project.knownActors"
                    rows="3"
                    class="w-full rounded-xl border border-slate-200 bg-white px-4 py-3 text-slate-800 shadow-inner shadow-slate-100 focus:border-primary-500 focus:outline-none focus:ring-2 focus:ring-primary-200"
                    placeholder="Lista separada por líneas"
                  />
                </div>
              </div>

              <div class="grid gap-6 md:grid-cols-2">
                <div class="space-y-4">
                  <div class="flex items-center justify-between">
                    <h3 class="text-sm font-semibold uppercase tracking-wide text-slate-600">Próximos hitos</h3>
                    <button
                      type="button"
                      class="text-sm font-semibold text-primary-600 hover:text-primary-500"
                      @click="addMilestone"
                    >
                      Añadir hito
                    </button>
                  </div>
                  <p class="text-xs text-slate-500">
                    Describe hitos clave y el horizonte temporal previsto.
                  </p>
                  <div v-if="project.milestones.length === 0" class="rounded-xl border border-dashed border-slate-300 p-4 text-sm text-slate-500">
                    Aún no hay hitos registrados.
                  </div>
                  <div v-for="milestone in project.milestones" :key="milestone.id" class="rounded-2xl border border-slate-200 bg-white p-4 shadow-sm">
                    <div class="space-y-3">
                      <div class="flex items-center gap-3">
                        <span class="inline-flex h-8 w-8 items-center justify-center rounded-full bg-primary-100 text-sm font-semibold text-primary-600">
                          {{ milestone.date ? milestone.date.slice(-2) : '··' }}
                        </span>
                        <input
                          v-model="milestone.label"
                          type="text"
                          class="flex-1 rounded-lg border border-slate-200 px-3 py-2 text-sm text-slate-800 focus:border-primary-500 focus:outline-none focus:ring-2 focus:ring-primary-200"
                          placeholder="Nombre del hito"
                        />
                      </div>
                      <div class="flex items-center justify-between gap-3">
                        <label class="text-xs font-medium uppercase tracking-wide text-slate-500">Año / fecha</label>
                        <input
                          v-model="milestone.date"
                          type="text"
                          class="w-32 rounded-lg border border-slate-200 px-3 py-2 text-sm text-slate-800 focus:border-primary-500 focus:outline-none focus:ring-2 focus:ring-primary-200"
                          placeholder="2025"
                        />
                      </div>
                      <button
                        type="button"
                        class="w-full rounded-lg bg-slate-100 py-2 text-sm font-medium text-slate-600 hover:bg-slate-200"
                        @click="removeMilestone(milestone.id)"
                      >
                        Quitar hito
                      </button>
                    </div>
                  </div>
                </div>

                <div class="space-y-4">
                  <div class="flex items-center justify-between">
                    <h3 class="text-sm font-semibold uppercase tracking-wide text-slate-600">Enlaces de referencia</h3>
                    <button
                      type="button"
                      class="text-sm font-semibold text-primary-600 hover:text-primary-500"
                      @click="addReference"
                    >
                      Añadir enlace
                    </button>
                  </div>
                  <p class="text-xs text-slate-500">Apunta fuentes, documentos o notas clave.</p>
                  <div v-if="project.references.length === 0" class="rounded-xl border border-dashed border-slate-300 p-4 text-sm text-slate-500">
                    Aún no hay referencias.
                  </div>
                  <div v-for="reference in project.references" :key="reference.id" class="rounded-2xl border border-slate-200 bg-white p-4 shadow-sm">
                    <div class="space-y-3">
                      <input
                        v-model="reference.label"
                        type="text"
                        class="w-full rounded-lg border border-slate-200 px-3 py-2 text-sm text-slate-800 focus:border-primary-500 focus:outline-none focus:ring-2 focus:ring-primary-200"
                        placeholder="Nombre de la referencia"
                      />
                      <input
                        v-model="reference.url"
                        type="text"
                        class="w-full rounded-lg border border-slate-200 px-3 py-2 text-sm text-slate-800 focus:border-primary-500 focus:outline-none focus:ring-2 focus:ring-primary-200"
                        placeholder="URL o ruta"
                      />
                      <button
                        type="button"
                        class="w-full rounded-lg bg-slate-100 py-2 text-sm font-medium text-slate-600 hover:bg-slate-200"
                        @click="removeReference(reference.id)"
                      >
                        Quitar referencia
                      </button>
                    </div>
                  </div>
                </div>
              </div>
            </form>
          </section>

          <section v-else-if="currentStep === 1" class="space-y-8">
            <div class="flex flex-col gap-4 rounded-2xl border border-slate-200 bg-slate-50/60 p-6 md:flex-row md:items-center md:justify-between">
              <div>
                <h2 class="text-xl font-semibold text-slate-800">Paso 2 · Afinar supuestos</h2>
                <p class="mt-1 text-sm text-slate-600">
                  Valida criterios técnicos y estratégicos antes de profundizar en el mapeo. Responde cada pregunta o utiliza el
                  auto rellenado del caso de Níjar.
                </p>
              </div>
              <button
                type="button"
                @click="autofillDiscovery"
                class="inline-flex items-center justify-center gap-2 rounded-full bg-primary-600 px-5 py-2 text-sm font-semibold text-white shadow-lg shadow-primary-600/40 transition hover:bg-primary-500"
              >
                <span class="inline-block h-2 w-2 rounded-full bg-white" />
                Auto rellenar respuestas demo
              </button>
            </div>

            <div class="grid gap-6">
              <article
                v-for="question in discoveryQuestions"
                :key="question.id"
                class="rounded-3xl border border-slate-200 bg-white p-6 shadow-sm transition hover:shadow-md"
              >
                <h3 class="text-base font-semibold text-slate-800">{{ question.prompt }}</h3>
                <textarea
                  v-model="question.answer"
                  rows="3"
                  class="mt-4 w-full rounded-2xl border border-slate-200 bg-slate-50 px-4 py-3 text-sm text-slate-800 shadow-inner shadow-slate-100 focus:border-primary-500 focus:bg-white focus:outline-none focus:ring-2 focus:ring-primary-200"
                  placeholder="Redacta la mejor respuesta disponible"
                />
              </article>
            </div>
          </section>

          <section v-else-if="currentStep === 2" class="space-y-10">
            <div class="flex flex-col gap-4 rounded-2xl border border-slate-200 bg-slate-50/60 p-6 md:flex-row md:items-center md:justify-between">
              <div>
                <h2 class="text-xl font-semibold text-slate-800">Paso 3 · Identificación y priorización</h2>
                <p class="mt-1 text-sm text-slate-600">
                  Con las respuestas anteriores proponemos un listado inicial editable. Ajusta, elimina o incorpora actores para afinar tu mapa.
                </p>
              </div>
              <div class="flex flex-col items-start gap-3 md:items-end">
                <div class="rounded-full bg-white px-4 py-2 text-sm font-semibold text-primary-600 shadow-inner shadow-primary-200">
                  {{ stakeholderCount }} actores registrados
                </div>
                <button
                  type="button"
                  class="inline-flex items-center gap-2 text-xs font-semibold text-primary-600 hover:text-primary-500"
                  @click="loadStakeholderSuggestions"
                >
                  Regenerar propuesta base
                </button>
              </div>
            </div>

            <form class="grid gap-6 rounded-2xl border border-slate-200 bg-white p-6 shadow-lg" @submit.prevent="addStakeholder">
              <div class="grid gap-6 md:grid-cols-2">
                <div class="space-y-2">
                  <label class="text-sm font-semibold text-slate-700">Nombre del actor o colectivo</label>
                  <input
                    v-model="newStakeholder.name"
                    type="text"
                    class="w-full rounded-xl border border-slate-200 bg-white px-4 py-3 text-slate-800 shadow-inner shadow-slate-100 focus:border-primary-500 focus:outline-none focus:ring-2 focus:ring-primary-200"
                    placeholder="Ej. Comunidad de regantes"
                    required
                  />
                </div>
                <div class="space-y-2">
                  <label class="text-sm font-semibold text-slate-700">Tipo</label>
                  <input
                    v-model="newStakeholder.type"
                    type="text"
                    class="w-full rounded-xl border border-slate-200 bg-white px-4 py-3 text-slate-800 shadow-inner shadow-slate-100 focus:border-primary-500 focus:outline-none focus:ring-2 focus:ring-primary-200"
                    placeholder="Entidad, persona, agencia..."
                  />
                </div>
                <div class="space-y-2">
                  <label class="text-sm font-semibold text-slate-700">Categoría</label>
                  <input
                    v-model="newStakeholder.category"
                    type="text"
                    class="w-full rounded-xl border border-slate-200 bg-white px-4 py-3 text-slate-800 shadow-inner shadow-slate-100 focus:border-primary-500 focus:outline-none focus:ring-2 focus:ring-primary-200"
                    placeholder="Administración, empresa, ONG..."
                  />
                </div>
                <div class="space-y-2">
                  <label class="text-sm font-semibold text-slate-700">Organización / ámbito</label>
                  <input
                    v-model="newStakeholder.organization"
                    type="text"
                    class="w-full rounded-xl border border-slate-200 bg-white px-4 py-3 text-slate-800 shadow-inner shadow-slate-100 focus:border-primary-500 focus:outline-none focus:ring-2 focus:ring-primary-200"
                    placeholder="Territorio, institución o grupo"
                  />
                </div>
                <div class="md:col-span-2 space-y-2">
                  <label class="text-sm font-semibold text-slate-700">Rol o función en el caso</label>
                  <textarea
                    v-model="newStakeholder.role"
                    rows="2"
                    class="w-full rounded-xl border border-slate-200 bg-white px-4 py-3 text-slate-800 shadow-inner shadow-slate-100 focus:border-primary-500 focus:outline-none focus:ring-2 focus:ring-primary-200"
                    placeholder="Implicación esperada, responsabilidades o motivaciones"
                  />
                </div>
                <div class="space-y-2">
                  <label class="text-sm font-semibold text-slate-700">Influencia</label>
                  <select
                    v-model="newStakeholder.influence"
                    class="w-full rounded-xl border border-slate-200 bg-white px-4 py-3 text-slate-800 focus:border-primary-500 focus:outline-none focus:ring-2 focus:ring-primary-200"
                  >
                    <option v-for="option in interestOptions" :key="option.value" :value="option.value">
                      {{ option.label }}
                    </option>
                  </select>
                </div>
                <div class="space-y-2">
                  <label class="text-sm font-semibold text-slate-700">Interés</label>
                  <select
                    v-model="newStakeholder.interest"
                    class="w-full rounded-xl border border-slate-200 bg-white px-4 py-3 text-slate-800 focus:border-primary-500 focus:outline-none focus:ring-2 focus:ring-primary-200"
                  >
                    <option v-for="option in interestOptions" :key="`interest-${option.value}`" :value="option.value">
                      {{ option.label }}
                    </option>
                  </select>
                </div>
                <div class="space-y-2">
                  <label class="text-sm font-semibold text-slate-700">Actitud</label>
                  <select
                    v-model="newStakeholder.attitude"
                    class="w-full rounded-xl border border-slate-200 bg-white px-4 py-3 text-slate-800 focus:border-primary-500 focus:outline-none focus:ring-2 focus:ring-primary-200"
                  >
                    <option v-for="attitude in attitudeOptions" :key="attitude" :value="attitude">
                      {{ attitude }}
                    </option>
                  </select>
                </div>
                <div class="md:col-span-2 space-y-2">
                  <label class="text-sm font-semibold text-slate-700">Notas internas</label>
                  <textarea
                    v-model="newStakeholder.notes"
                    rows="2"
                    class="w-full rounded-xl border border-slate-200 bg-white px-4 py-3 text-slate-800 shadow-inner shadow-slate-100 focus:border-primary-500 focus:outline-none focus:ring-2 focus:ring-primary-200"
                    placeholder="Motivaciones, riesgos u oportunidades adicionales"
                  />
                </div>
              </div>
              <div class="flex items-center justify-end gap-4">
                <p class="text-xs text-slate-500">Guarda cada actor para construir el mapa.</p>
                <button
                  type="submit"
                  class="inline-flex items-center justify-center gap-2 rounded-full bg-primary-600 px-6 py-2 text-sm font-semibold text-white shadow-lg shadow-primary-600/40 transition hover:bg-primary-500"
                >
                  Añadir actor
                </button>
              </div>
            </form>

            <div v-if="stakeholderCount" class="grid gap-6 md:grid-cols-2">
              <article
                v-for="stakeholder in stakeholderList"
                :key="stakeholder.id"
                class="rounded-3xl border border-slate-200 bg-slate-50 p-6 shadow-sm transition hover:-translate-y-1 hover:shadow-xl"
              >
                <div class="flex items-start justify-between gap-4">
                  <div class="flex flex-wrap items-center gap-2 text-xs text-slate-500">
                    <span class="inline-flex items-center rounded-full bg-slate-200/60 px-2 py-0.5 font-semibold uppercase tracking-wide">Influ. {{ levelLabels[stakeholder.influence] }}</span>
                    <span class="inline-flex items-center rounded-full bg-slate-200/60 px-2 py-0.5 font-semibold uppercase tracking-wide">Interés {{ levelLabels[stakeholder.interest] }}</span>
                  </div>
                  <button
                    type="button"
                    class="inline-flex items-center rounded-full border border-red-200 px-3 py-1 text-xs font-semibold uppercase tracking-wide text-red-500 transition hover:border-red-300 hover:bg-red-50"
                    @click="removeStakeholder(stakeholder.id)"
                  >
                    Eliminar
                  </button>
                </div>

                <div class="mt-6 grid gap-5 text-sm text-slate-700 md:grid-cols-2">
                  <div class="space-y-2">
                    <label class="text-xs font-semibold uppercase tracking-wide text-slate-500">Stakeholder</label>
                    <input
                      v-model="stakeholder.name"
                      type="text"
                      class="w-full rounded-lg border border-slate-200 bg-white px-3 py-2 focus:border-primary-500 focus:outline-none focus:ring-2 focus:ring-primary-200"
                    />
                  </div>
                  <div class="space-y-2">
                    <label class="text-xs font-semibold uppercase tracking-wide text-slate-500">Tipo</label>
                    <input
                      v-model="stakeholder.type"
                      type="text"
                      class="w-full rounded-lg border border-slate-200 bg-white px-3 py-2 focus:border-primary-500 focus:outline-none focus:ring-2 focus:ring-primary-200"
                    />
                  </div>
                  <div class="space-y-2">
                    <label class="text-xs font-semibold uppercase tracking-wide text-slate-500">Categoría</label>
                    <input
                      v-model="stakeholder.category"
                      type="text"
                      class="w-full rounded-lg border border-slate-200 bg-white px-3 py-2 focus:border-primary-500 focus:outline-none focus:ring-2 focus:ring-primary-200"
                    />
                  </div>
                  <div class="space-y-2">
                    <label class="text-xs font-semibold uppercase tracking-wide text-slate-500">Organización / ámbito</label>
                    <input
                      v-model="stakeholder.organization"
                      type="text"
                      class="w-full rounded-lg border border-slate-200 bg-white px-3 py-2 focus:border-primary-500 focus:outline-none focus:ring-2 focus:ring-primary-200"
                    />
                  </div>
                  <div class="md:col-span-2 space-y-2">
                    <label class="text-xs font-semibold uppercase tracking-wide text-slate-500">Rol o función en el caso</label>
                    <textarea
                      v-model="stakeholder.role"
                      rows="2"
                      class="w-full rounded-lg border border-slate-200 bg-white px-3 py-2 focus:border-primary-500 focus:outline-none focus:ring-2 focus:ring-primary-200"
                    />
                  </div>
                  <div class="space-y-2">
                    <label class="text-xs font-semibold uppercase tracking-wide text-slate-500">Influencia</label>
                    <select
                      v-model="stakeholder.influence"
                      class="w-full rounded-lg border border-slate-200 bg-white px-3 py-2 focus:border-primary-500 focus:outline-none focus:ring-2 focus:ring-primary-200"
                    >
                      <option v-for="option in interestOptions" :key="`stakeholder-${stakeholder.id}-influence-${option.value}`" :value="option.value">
                        {{ option.label }}
                      </option>
                    </select>
                  </div>
                  <div class="space-y-2">
                    <label class="text-xs font-semibold uppercase tracking-wide text-slate-500">Interés</label>
                    <select
                      v-model="stakeholder.interest"
                      class="w-full rounded-lg border border-slate-200 bg-white px-3 py-2 focus:border-primary-500 focus:outline-none focus:ring-2 focus:ring-primary-200"
                    >
                      <option v-for="option in interestOptions" :key="`stakeholder-${stakeholder.id}-interest-${option.value}`" :value="option.value">
                        {{ option.label }}
                      </option>
                    </select>
                  </div>
                  <div class="space-y-2">
                    <label class="text-xs font-semibold uppercase tracking-wide text-slate-500">Actitud</label>
                    <select
                      v-model="stakeholder.attitude"
                      class="w-full rounded-lg border border-slate-200 bg-white px-3 py-2 focus:border-primary-500 focus:outline-none focus:ring-2 focus:ring-primary-200"
                    >
                      <option v-for="attitude in attitudeOptions" :key="`stakeholder-${stakeholder.id}-attitude-${attitude}`" :value="attitude">
                        {{ attitude }}
                      </option>
                    </select>
                  </div>
                  <div class="md:col-span-2 space-y-2">
                    <label class="text-xs font-semibold uppercase tracking-wide text-slate-500">Notas internas</label>
                    <textarea
                      v-model="stakeholder.notes"
                      rows="2"
                      class="w-full rounded-lg border border-slate-200 bg-white px-3 py-2 focus:border-primary-500 focus:outline-none focus:ring-2 focus:ring-primary-200"
                    />
                  </div>
                </div>
              </article>
            </div>
            <div v-else class="rounded-3xl border border-dashed border-slate-300 p-10 text-center text-slate-500">
              Agrega al menos un actor para habilitar el mapa.
            </div>
          </section>

          <section v-else class="space-y-10">
            <div class="flex flex-col gap-4 rounded-2xl border border-slate-200 bg-slate-50/60 p-6 md:flex-row md:items-center md:justify-between">
              <div>
                <h2 class="text-xl font-semibold text-slate-800">Paso 4 · Visualización y narrativa</h2>
                <p class="mt-1 text-sm text-slate-600">
                  Analiza el mapa posicionando a cada actor según su influencia e interés y conecta la historia con los hitos
                  del proyecto.
                </p>
              </div>
              <button
                type="button"
                class="rounded-full border border-primary-200 px-4 py-2 text-sm font-semibold text-primary-600 hover:bg-primary-50"
                @click="resetAll"
              >
                Iniciar un nuevo proyecto
              </button>
            </div>

            <div class="grid gap-8 lg:grid-cols-[1.4fr_1fr]">
              <article class="rounded-3xl border border-slate-200 bg-white p-8 shadow-xl">
                <h3 class="text-lg font-semibold text-slate-800">Resumen del proyecto</h3>
                <dl class="mt-6 grid gap-4 text-sm text-slate-700 md:grid-cols-2">
                  <div>
                    <dt class="font-semibold text-slate-600">Empresa promotora</dt>
                    <dd class="mt-1 text-slate-800">{{ project.sponsor || '—' }}</dd>
                  </div>
                  <div>
                    <dt class="font-semibold text-slate-600">Fase actual</dt>
                    <dd class="mt-1 text-slate-800">{{ project.phase || '—' }}</dd>
                  </div>
                  <div class="md:col-span-2">
                    <dt class="font-semibold text-slate-600">Descripción</dt>
                    <dd class="mt-1 whitespace-pre-line text-slate-800">{{ project.description || '—' }}</dd>
                  </div>
                  <div>
                    <dt class="font-semibold text-slate-600">Localización</dt>
                    <dd class="mt-1 text-slate-800">{{ project.location || '—' }}</dd>
                  </div>
                  <div>
                    <dt class="font-semibold text-slate-600">Objetivo principal</dt>
                    <dd class="mt-1 text-slate-800">{{ project.mainObjective || '—' }}</dd>
                  </div>
                  <div class="md:col-span-2">
                    <dt class="font-semibold text-slate-600">Temas sensibles</dt>
                    <dd class="mt-1 whitespace-pre-line text-slate-800">{{ project.sensitiveTopics || '—' }}</dd>
                  </div>
                  <div class="md:col-span-2">
                    <dt class="font-semibold text-slate-600">Actores iniciales conocidos</dt>
                    <dd class="mt-1 whitespace-pre-line text-slate-800">{{ project.knownActors || '—' }}</dd>
                  </div>
                </dl>

                <div class="mt-8 grid gap-6 md:grid-cols-2">
                  <div>
                    <h4 class="text-sm font-semibold uppercase tracking-wide text-slate-500">Próximos hitos</h4>
                    <ul class="mt-3 space-y-3 text-sm text-slate-700">
                      <li v-if="project.milestones.length === 0" class="rounded-lg bg-slate-100 px-3 py-2 text-slate-500">
                        Sin hitos registrados.
                      </li>
                      <li v-for="milestone in project.milestones" :key="milestone.id" class="rounded-lg border border-slate-200 bg-white px-3 py-2 shadow-sm">
                        <p class="font-semibold text-primary-600">{{ milestone.label || 'Hito sin nombre' }}</p>
                        <p class="text-xs text-slate-500">{{ milestone.date || 'Fecha por definir' }}</p>
                      </li>
                    </ul>
                  </div>
                  <div>
                    <h4 class="text-sm font-semibold uppercase tracking-wide text-slate-500">Referencias</h4>
                    <ul class="mt-3 space-y-3 text-sm text-slate-700">
                      <li v-if="project.references.length === 0" class="rounded-lg bg-slate-100 px-3 py-2 text-slate-500">
                        Sin referencias registradas.
                      </li>
                      <li v-for="reference in project.references" :key="reference.id" class="rounded-lg border border-slate-200 bg-white px-3 py-2 shadow-sm">
                        <p class="font-semibold text-primary-600">{{ reference.label || 'Referencia sin título' }}</p>
                        <a
                          v-if="reference.url"
                          :href="reference.url"
                          target="_blank"
                          rel="noopener"
                          class="mt-1 inline-flex items-center gap-1 text-xs text-slate-500 hover:text-primary-600"
                        >
                          {{ reference.url }}
                        </a>
                      </li>
                    </ul>
                  </div>
                </div>
              </article>

              <article class="rounded-3xl border border-slate-200 bg-white p-8 shadow-xl">
                <h3 class="text-lg font-semibold text-slate-800">Mapa de interés vs. influencia</h3>
                <p class="mt-2 text-sm text-slate-600">
                  Cada cuadrante agrupa actores según su poder de decisión y el grado de atención que prestan al proyecto.
                </p>
                <div class="mt-6 overflow-x-auto">
                  <div class="min-w-[480px]">
                    <div class="grid grid-cols-4 gap-3 text-sm">
                      <div />
                      <div
                        v-for="interest in interestOrder"
                        :key="`header-${interest}`"
                        class="rounded-2xl bg-slate-100 px-3 py-2 text-center font-semibold text-slate-600"
                      >
                        Interés {{ levelLabels[interest] }}
                      </div>
                      <template v-for="(row, rowIndex) in matrix" :key="`row-${rowIndex}`">
                        <div class="flex items-center justify-center rounded-2xl bg-slate-100 px-3 py-4 text-center text-sm font-semibold text-slate-600">
                          Influencia {{ levelLabels[influenceOrder[rowIndex]] }}
                        </div>
                        <div
                          v-for="cell in row"
                          :key="`cell-${cell.influence}-${cell.interest}`"
                          class="min-h-[120px] rounded-3xl border border-dashed border-slate-300 bg-slate-50 p-3"
                        >
                          <p class="text-xs font-semibold uppercase tracking-wide text-slate-500">
                            {{ cell.stakeholders.length }} actor(es)
                          </p>
                          <ul class="mt-2 space-y-2">
                            <li
                              v-for="stakeholder in cell.stakeholders"
                              :key="stakeholder.id"
                              class="rounded-2xl bg-white px-3 py-2 text-xs text-slate-600 shadow-sm"
                            >
                              <p class="font-semibold text-slate-800">{{ stakeholder.name }}</p>
                              <p class="mt-1 flex flex-wrap gap-1 text-[11px] uppercase tracking-wide text-primary-600">
                                <span>{{ stakeholder.type || 'Tipo no definido' }}</span>
                                <span v-if="stakeholder.category" class="text-slate-400">·</span>
                                <span v-if="stakeholder.category">{{ stakeholder.category }}</span>
                              </p>
                              <p v-if="stakeholder.organization" class="mt-1 text-[11px] text-slate-500">
                                Organización: {{ stakeholder.organization }}
                              </p>
                              <p v-if="stakeholder.role" class="mt-1 text-[11px] text-slate-500">
                                Rol: {{ stakeholder.role }}
                              </p>
                              <p class="mt-2 text-[11px] font-semibold uppercase tracking-wide text-primary-600">
                                Actitud: {{ stakeholder.attitude }}
                              </p>
                              <p v-if="stakeholder.notes" class="mt-1 text-[11px] text-slate-400">
                                Nota interna: {{ stakeholder.notes }}
                              </p>
                            </li>
                          </ul>
                          <p v-if="!cell.stakeholders.length" class="mt-6 text-center text-[11px] italic text-slate-400">
                            Sin actores registrados
                          </p>
                        </div>
                      </template>
                    </div>
                  </div>
                </div>
                <div class="mt-6 rounded-2xl bg-primary-50 p-4 text-sm text-primary-700">
                  <p class="font-semibold">Siguiente paso recomendado</p>
                  <p class="mt-1 text-primary-600">
                    Prioriza a los actores con alta influencia o interés e identifica acciones de relacionamiento específicas
                    para los próximos hitos del cronograma.
                  </p>
                </div>
              </article>
            </div>
          </section>
        </div>

        <div class="flex flex-col gap-4 border-t border-slate-200 bg-slate-50 px-8 py-6 text-sm text-slate-600 md:flex-row md:items-center md:justify-between">
          <div class="font-medium">
            {{ steps[currentStep].title }}
            <span class="text-slate-400">· {{ steps[currentStep].subtitle }}</span>
          </div>
          <div class="flex flex-wrap items-center gap-3">
            <button
              type="button"
              class="rounded-full border border-slate-300 px-4 py-2 font-semibold text-slate-600 transition hover:border-slate-400 hover:bg-white"
              :disabled="currentStep === 0"
              :class="currentStep === 0 ? 'opacity-40 cursor-not-allowed' : ''"
              @click="previousStep"
            >
              Paso anterior
            </button>
            <button
              v-if="currentStep < steps.length - 1"
              type="button"
              class="rounded-full bg-primary-600 px-6 py-2 font-semibold text-white shadow-lg shadow-primary-600/30 transition hover:bg-primary-500 disabled:cursor-not-allowed disabled:opacity-60"
              :disabled="!canAdvance"
              @click="nextStep"
            >
              {{ nextButtonLabel }}
            </button>
            <button
              v-else
              type="button"
              class="rounded-full bg-primary-700 px-6 py-2 font-semibold text-white shadow-lg shadow-primary-700/30 transition hover:bg-primary-600"
              @click="resetAll"
            >
              Generar nuevo escenario
            </button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
