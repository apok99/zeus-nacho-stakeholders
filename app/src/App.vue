<script setup>
import { computed, reactive, ref } from 'vue'

const steps = [
  {
    id: 'project',
    title: 'Definición del proyecto',
    subtitle: 'Contexto y alcance inicial',
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

let milestoneId = 0
let referenceId = 0
let stakeholderId = 0

const newStakeholder = reactive({
  name: '',
  segment: '',
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
    return stakeholders.value.length > 0
  }

  return true
})

const progressBarStyle = computed(() => ({
  width: `${progressPercentage.value}%`,
}))

const matrix = computed(() => {
  return influenceOrder.map((influence) => {
    return interestOrder.map((interest) => ({
      influence,
      interest,
      stakeholders: stakeholders.value.filter(
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
    segment: data.segment ?? '',
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

  stakeholders.value.push(
    createStakeholder({
      ...newStakeholder,
    }),
  )

  newStakeholder.name = ''
  newStakeholder.segment = ''
  newStakeholder.influence = 'medium'
  newStakeholder.interest = 'medium'
  newStakeholder.attitude = 'Neutral'
  newStakeholder.notes = ''
}

function removeStakeholder(id) {
  const index = stakeholders.value.findIndex((item) => item.id === id)
  if (index >= 0) {
    stakeholders.value.splice(index, 1)
  }
}

function nextStep() {
  if (currentStep.value < steps.length - 1 && canAdvance.value) {
    currentStep.value += 1
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

function resetAll() {
  resetProject()
  stakeholders.value = []
  currentStep.value = 0
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

  stakeholders.value = [
    createStakeholder({
      name: 'Ministerio para la Transición Ecológica y Reto Demográfico',
      segment: 'Administración central',
      influence: 'high',
      interest: 'high',
      attitude: 'Apoyo condicional',
      notes: 'Órgano competente para la evaluación ambiental y autorizaciones clave.',
    }),
    createStakeholder({
      name: 'Junta de Andalucía (Consejería de Agricultura, Pesca, Agua y Desarrollo Rural)',
      segment: 'Gobierno autonómico',
      influence: 'high',
      interest: 'high',
      attitude: 'Apoyo condicional',
      notes: 'Definición de políticas hídricas regionales y apoyo a regantes.',
    }),
    createStakeholder({
      name: 'Ayuntamiento de Níjar',
      segment: 'Gobierno local',
      influence: 'medium',
      interest: 'high',
      attitude: 'Apoyo cauteloso',
      notes: 'Impacto urbanístico y expectativas de empleo en el municipio.',
    }),
    createStakeholder({
      name: 'Sindicatos agrarios (COAG, ASAJA)',
      segment: 'Sector agrario organizado',
      influence: 'medium',
      interest: 'high',
      attitude: 'Apoyo',
      notes: 'Garantizar suministro hídrico para la agricultura intensiva.',
    }),
    createStakeholder({
      name: 'Cooperativas agrícolas locales',
      segment: 'Productores locales',
      influence: 'medium',
      interest: 'high',
      attitude: 'Apoyo',
      notes: 'Beneficiarios directos del incremento en disponibilidad de agua.',
    }),
    createStakeholder({
      name: 'Asociaciones ecologistas (Ecologistas en Acción, Greenpeace)',
      segment: 'ONG ambiental',
      influence: 'medium',
      interest: 'high',
      attitude: 'Oposición',
      notes: 'Preocupaciones por impacto costero y consumo energético.',
    }),
    createStakeholder({
      name: 'Empresas exportadoras de frutas y hortalizas',
      segment: 'Sector privado',
      influence: 'medium',
      interest: 'medium',
      attitude: 'Apoyo condicional',
      notes: 'Dependen del suministro para sostener contratos internacionales.',
    }),
    createStakeholder({
      name: 'Medios regionales (La Voz de Almería)',
      segment: 'Medios de comunicación',
      influence: 'low',
      interest: 'medium',
      attitude: 'Vigilante',
      notes: 'Contribuyen a formar opinión pública regional.',
    }),
  ]
}
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

          <section v-else-if="currentStep === 1" class="space-y-10">
            <div class="flex flex-col gap-4 rounded-2xl border border-slate-200 bg-slate-50/60 p-6 md:flex-row md:items-center md:justify-between">
              <div>
                <h2 class="text-xl font-semibold text-slate-800">Paso 2 · Identificación y priorización</h2>
                <p class="mt-1 text-sm text-slate-600">
                  Registra a cada actor, asigna su nivel de influencia e interés, e identifica la actitud predominante.
                </p>
              </div>
              <div class="rounded-full bg-white px-4 py-2 text-sm font-semibold text-primary-600 shadow-inner shadow-primary-200">
                {{ stakeholders.value.length }} actores registrados
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
                  <label class="text-sm font-semibold text-slate-700">Segmento / categoría</label>
                  <input
                    v-model="newStakeholder.segment"
                    type="text"
                    class="w-full rounded-xl border border-slate-200 bg-white px-4 py-3 text-slate-800 shadow-inner shadow-slate-100 focus:border-primary-500 focus:outline-none focus:ring-2 focus:ring-primary-200"
                    placeholder="Administración, empresa, ONG..."
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
                <div class="space-y-2">
                  <label class="text-sm font-semibold text-slate-700">Notas clave</label>
                  <textarea
                    v-model="newStakeholder.notes"
                    rows="3"
                    class="w-full rounded-xl border border-slate-200 bg-white px-4 py-3 text-slate-800 shadow-inner shadow-slate-100 focus:border-primary-500 focus:outline-none focus:ring-2 focus:ring-primary-200"
                    placeholder="Motivaciones, riesgos u oportunidades"
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

            <div v-if="stakeholders.value.length" class="grid gap-6 md:grid-cols-2">
              <article
                v-for="stakeholder in stakeholders.value"
                :key="stakeholder.id"
                class="group relative rounded-3xl border border-slate-200 bg-slate-50 p-6 shadow-sm transition hover:-translate-y-1 hover:shadow-xl"
              >
                <div class="absolute right-4 top-4 flex items-center gap-2 text-xs text-slate-400">
                  <span class="inline-flex items-center rounded-full bg-slate-200/60 px-2 py-0.5 font-semibold uppercase tracking-wide">Influ. {{ levelLabels[stakeholder.influence] }}</span>
                  <span class="inline-flex items-center rounded-full bg-slate-200/60 px-2 py-0.5 font-semibold uppercase tracking-wide">Interés {{ levelLabels[stakeholder.interest] }}</span>
                </div>
                <h3 class="pr-24 text-lg font-semibold text-slate-800">{{ stakeholder.name }}</h3>
                <p class="mt-2 text-sm font-medium uppercase tracking-wide text-primary-600">
                  {{ stakeholder.segment || 'Segmento no definido' }}
                </p>
                <p class="mt-3 text-sm text-slate-600" v-if="stakeholder.notes">{{ stakeholder.notes }}</p>
                <div class="mt-4 inline-flex rounded-full bg-white px-3 py-1 text-xs font-semibold uppercase tracking-wide text-slate-500">
                  Actitud: <span class="ml-2 text-primary-600">{{ stakeholder.attitude }}</span>
                </div>
                <button
                  type="button"
                  class="mt-6 w-full rounded-full border border-red-200 bg-white py-2 text-sm font-semibold text-red-500 transition hover:border-red-300 hover:bg-red-50"
                  @click="removeStakeholder(stakeholder.id)"
                >
                  Eliminar actor
                </button>
              </article>
            </div>
            <div v-else class="rounded-3xl border border-dashed border-slate-300 p-10 text-center text-slate-500">
              Agrega al menos un actor para habilitar el mapa.
            </div>
          </section>

          <section v-else class="space-y-10">
            <div class="flex flex-col gap-4 rounded-2xl border border-slate-200 bg-slate-50/60 p-6 md:flex-row md:items-center md:justify-between">
              <div>
                <h2 class="text-xl font-semibold text-slate-800">Paso 3 · Visualización y narrativa</h2>
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
                              <p class="text-[11px] text-primary-600">{{ stakeholder.attitude }}</p>
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
              Continuar
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
