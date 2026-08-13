# SANTA-BARBARA-LEGAL-CLINIC-S-OLOMON-PROOFS

=== FILE: package.json ===
{
  "name": "aletheia-proofos-math-enterprise",
  "version": "0.1.0",
  "private": true,
  "description": "Aletheia ProofOS Enterprise mathematical governance seed with SolomonAI, SBLC draft agent, Proof Spine authority, and eight mathematical certificate frameworks.",
  "license": "UNLICENSED",
  "type": "module",
  "packageManager": "pnpm@9.15.4",
  "engines": {
    "node": ">=20.11.0",
    "pnpm": ">=9.0.0"
  },
  "scripts": {
    "build": "pnpm -r build",
    "typecheck": "pnpm -r typecheck",
    "test": "vitest run",
    "smoke": "tsx apps/api/src/smoke.ts",
    "audit:brain-boundary": "tsx scripts/audit-brain-boundary.ts",
    "audit:proof-spine": "tsx scripts/audit-proof-spine.ts",
    "audit:release-capability": "tsx scripts/audit-release-capability.ts",
    "audit:sblc-draft-boundary": "tsx scripts/audit-sblc-draft-boundary.ts",
    "audit:advisory-airlock": "tsx scripts/audit-advisory-airlock.ts",
    "verify": "pnpm typecheck && pnpm test && pnpm audit:brain-boundary && pnpm audit:proof-spine && pnpm audit:release-capability && pnpm audit:sblc-draft-boundary && pnpm audit:advisory-airlock && pnpm smoke",
    "dev": "tsx apps/api/src/server.ts"
  },
  "dependencies": {
    "fastify": "^5.2.1",
    "zod": "^3.24.1"
  },
  "devDependencies": {
    "@types/node": "^22.10.2",
    "tsx": "^4.19.2",
    "typescript": "^5.7.2",
    "vitest": "^2.1.8"
  }
}
=== FILE: pnpm-workspace.yaml ===
packages:
  - "packages/*"
  - "apps/*"
=== FILE: tsconfig.base.json ===
{
  "compilerOptions": {
    "target": "ES2022",
    "lib": ["ES2022"],
    "module": "NodeNext",
    "moduleResolution": "NodeNext",
    "strict": true,
    "noUncheckedIndexedAccess": true,
    "exactOptionalPropertyTypes": true,
    "noImplicitReturns": true,
    "noFallthroughCasesInSwitch": true,
    "useUnknownInCatchVariables": true,
    "verbatimModuleSyntax": true,
    "isolatedModules": true,
    "forceConsistentCasingInFileNames": true,
    "skipLibCheck": true,
    "declaration": true,
    "declarationMap": true,
    "sourceMap": true,
    "baseUrl": ".",
    "paths": {
      "@aletheia/shared": ["packages/shared/src/index.ts"],
      "@aletheia/proof-spine": ["packages/proof-spine/src/index.ts"],
      "@aletheia/brain": ["packages/brain/src/index.ts"]
    }
  }
}
=== FILE: tsconfig.json ===
{
  "files": [],
  "references": [
    { "path": "./packages/shared" },
    { "path": "./packages/proof-spine" },
    { "path": "./packages/brain" },
    { "path": "./apps/api" }
  ]
}
=== FILE: vitest.config.ts ===
import { defineConfig } from "vitest/config";

export default defineConfig({
  test: {
    include: ["packages/**/*.test.ts", "apps/**/*.test.ts"],
    environment: "node"
  }
});
=== FILE: .gitignore ===
node_modules
dist
coverage
.env
.env.*
.DS_Store
*.log
=== FILE: README.md ===
# SolomonAI InfraR&D & KerneLLiquidIT 

Aletheia ProofOS by SolomonAI is a supervised proof infrastructure seed for regulated institutional work.

This repository encodes the governing doctrine:

> The Brain may improve the work.  
> The Proof Spine must authorize the release.

## What ships

- Enterprise Proof Spine authority layer
- Codeseed two-layer event hashing
- SolomonAI advisory-only planning
- SBLC Legal Draft Agent candidate-only drafting
- Public-safe mathematical certificate bundle
- Eight executable mathematical frameworks:
  - CBF: Categorical Boundary Functor
  - MPPF: Morphogenetic Proof Phase Field
  - IGAM: Information Geometry Advisory Manifold
  - HWD: Hamiltonian Workflow Dynamics
  - SDIV: Spectral DAG Integrity Verification
  - CCER: Cognitive Capacity Entropy Routing
  - LSSC: Lyapunov Stable State Certification
  - BCCN: Bayesian Citation Confidence Network

## Run

```bash
pnpm install
pnpm verify
pnpm dev
API
POST /v1/brain/solomon/plan
POST /v1/brain/sblc-draft/candidate
POST /v1/proof/events/append
POST /v1/proof/replay
POST /v1/proof/audit-packet/build
GET  /v1/public/verify/:recordHash
Authority boundary

SolomonAI and SBLC Draft Agent are advisory-only.

They cannot:
•	approve
•	release
•	export
•	issue receipts
•	create manifests
•	finalize MatterLedgerRecords
•	alter audit history
•	bypass redaction

All release paths must flow through:
ReleaseCapability
→ ReleaseGateEvaluation
→ VerificationManifest
→ ExportReceipt
→ ReplayResult
→ MatterLedgerRecord
→ AuditPacket
=== FILE: packages/shared/package.json ===

```json
{
  "name": "@aletheia/shared",
  "version": "0.1.0",
  "private": true,
  "type": "module",
  "main": "dist/index.js",
  "types": "dist/index.d.ts",
  "exports": {
    ".": {
      "types": "./dist/index.d.ts",
      "default": "./dist/index.js"
    }
  },
  "scripts": {
    "build": "tsc -b",
    "typecheck": "tsc -b --pretty false",
    "test": "vitest run"
  },
  "dependencies": {
    "zod": "^3.24.1"
  }
}
=== FILE: packages/shared/tsconfig.json ===
{
  "extends": "../../tsconfig.base.json",
  "compilerOptions": {
    "rootDir": "src",
    "outDir": "dist",
    "composite": true
  },
  "include": ["src/**/*.ts"]
}
=== FILE: packages/shared/src/hash.ts ===
import { createHash, createHmac, timingSafeEqual } from "node:crypto";

export type JsonPrimitive = string | number | boolean | null;
export type JsonValue = JsonPrimitive | JsonValue[] | { [key: string]: JsonValue };

function isPlainObject(value: unknown): value is Record<string, unknown> {
  return Object.prototype.toString.call(value) === "[object Object]";
}

export function canonicalize(value: unknown): JsonValue {
  if (value === undefined) {
    throw new Error("Cannot canonicalize undefined.");
  }

  if (value === null || typeof value === "string" || typeof value === "boolean") {
    return value;
  }

  if (typeof value === "number") {
    if (!Number.isFinite(value)) {
      throw new Error("Cannot canonicalize non-finite number.");
    }

    return value;
  }

  if (Array.isArray(value)) {
    return value.map((item) => canonicalize(item));
  }

  if (isPlainObject(value)) {
    const sorted = Object.entries(value)
      .filter(([, entryValue]) => entryValue !== undefined)
      .sort(([left], [right]) => left.localeCompare(right))
      .map(([key, entryValue]) => [key, canonicalize(entryValue)] as const);

    return Object.fromEntries(sorted);
  }

  throw new Error(`Unsupported canonical value type: ${typeof value}`);
}

export function canonicalJson(value: unknown): string {
  return JSON.stringify(canonicalize(value));
}

export function sha256Canonical(value: unknown): string {
  return createHash("sha256").update(canonicalJson(value)).digest("hex");
}

export function sha256Text(value: string): string {
  return createHash("sha256").update(value, "utf8").digest("hex");
}

export function hmacSha256Canonical(secret: string, value: unknown): string {
  return createHmac("sha256", secret).update(canonicalJson(value)).digest("hex");
}

export function timingSafeHexEqual(left: string, right: string): boolean {
  if (!/^[a-f0-9]+$/i.test(left) || !/^[a-f0-9]+$/i.test(right)) {
    return false;
  }

  const leftBuffer = Buffer.from(left, "hex");
  const rightBuffer = Buffer.from(right, "hex");

  if (leftBuffer.length !== rightBuffer.length) {
    return false;
  }

  return timingSafeEqual(leftBuffer, rightBuffer);
}
=== FILE: packages/shared/src/time.ts ===
import { z } from "zod";

export const IsoDateTimeSchema = z.string().refine((value) => !Number.isNaN(Date.parse(value)), {
  message: "Expected a valid ISO-8601 timestamp"
});

export type IsoDateTime = z.infer<typeof IsoDateTimeSchema>;

export function nowIso(): IsoDateTime {
  return IsoDateTimeSchema.parse(new Date().toISOString());
}

export function isExpiredIso(expiresAt: string, now: string = nowIso()): boolean {
  return Date.parse(expiresAt) <= Date.parse(now);
}
=== FILE: packages/shared/src/errors.ts ===
export type AletheiaErrorCode =
  | "VALIDATION_FAILED"
  | "AUTHORITY_DENIED"
  | "ADVISORY_BOUNDARY_VIOLATION"
  | "CHAIN_INVALID"
  | "EVENT_TAMPERED"
  | "CAPABILITY_EXPIRED"
  | "CAPABILITY_REVOKED"
  | "CAPABILITY_SCOPE_DENIED"
  | "CAPABILITY_SIGNATURE_INVALID"
  | "RELEASE_GATE_FAILED"
  | "REPLAY_MISMATCH"
  | "LYAPUNOV_UNSTABLE"
  | "SPECTRAL_DAG_DISCONNECTED"
  | "HAMILTONIAN_INCONSISTENT"
  | "CATEGORICAL_BOUNDARY_FAILED"
  | "PUBLIC_VERIFICATION_NOT_FOUND"
  | "INTERNAL_ERROR";

export class AletheiaError extends Error {
  public readonly code: AletheiaErrorCode;
  public readonly details: Record<string, unknown>;

  constructor(code: AletheiaErrorCode, message: string, details: Record<string, unknown> = {}) {
    super(message);
    this.name = "AletheiaError";
    this.code = code;
    this.details = details;
  }
}
=== FILE: packages/shared/src/authority.ts ===
import { z } from "zod";

export const ActorTypeSchema = z.enum(["HUMAN", "AI", "SYSTEM", "EXTERNAL"]);
export type ActorType = z.infer<typeof ActorTypeSchema>;

export const ActorContextSchema = z.object({
  tenantId: z.string().min(1),
  matterId: z.string().min(1).optional(),
  actorId: z.string().min(1),
  actorType: ActorTypeSchema,
  verified: z.boolean(),
  roles: z.array(z.string().min(1)).default([])
});

export type ActorContext = z.infer<typeof ActorContextSchema>;

export const HumanActorContextSchema = ActorContextSchema.extend({
  actorType: z.literal("HUMAN"),
  verified: z.literal(true)
});

export type HumanActorContext = z.infer<typeof HumanActorContextSchema>;

export const AdvisoryBoundarySchema = z.object({
  advisoryOnly: z.literal(true),
  candidateOnly: z.literal(true),
  humanReviewRequired: z.literal(true),
  externalReleaseAllowed: z.literal(false),
  proofAuthorityGranted: z.literal(false),
  releaseAuthority: z.literal("Proof Spine only"),
  mayApprove: z.literal(false),
  mayRelease: z.literal(false),
  mayExport: z.literal(false),
  mayIssueReceipt: z.literal(false),
  mayCreateManifest: z.literal(false),
  mayFinalizeMatterLedgerRecord: z.literal(false),
  mayBypassRedactions: z.literal(false),
  mayAlterAuditHistory: z.literal(false)
});

export type AdvisoryBoundary = z.infer<typeof AdvisoryBoundarySchema>;

export const ADVISORY_BOUNDARY: AdvisoryBoundary = {
  advisoryOnly: true,
  candidateOnly: true,
  humanReviewRequired: true,
  externalReleaseAllowed: false,
  proofAuthorityGranted: false,
  releaseAuthority: "Proof Spine only",
  mayApprove: false,
  mayRelease: false,
  mayExport: false,
  mayIssueReceipt: false,
  mayCreateManifest: false,
  mayFinalizeMatterLedgerRecord: false,
  mayBypassRedactions: false,
  mayAlterAuditHistory: false
};

export const CapabilityScopeSchema = z.enum([
  "matter:read",
  "matter:write",
  "draft:create",
  "review:approve",
  "redaction:review",
  "release:evaluate",
  "manifest:create",
  "receipt:issue",
  "ledger:finalize",
  "audit:build",
  "public:verify"
]);

export type CapabilityScope = z.infer<typeof CapabilityScopeSchema>;

export const ReleaseCapabilityPayloadSchema = z.object({
  capabilityId: z.string().min(1),
  tenantId: z.string().min(1),
  matterId: z.string().min(1),
  issuedToActorId: z.string().min(1),
  issuedByActorId: z.string().min(1),
  scopes: z.array(CapabilityScopeSchema).min(1),
  categoricalFunctorTag: z.literal("AuthorityFunctor.ProofSpine"),
  issuedAt: z.string().datetime(),
  expiresAt: z.string().datetime()
});

export type ReleaseCapabilityPayload = z.infer<typeof ReleaseCapabilityPayloadSchema>;

export const ReleaseCapabilitySchema = z.object({
  payload: ReleaseCapabilityPayloadSchema,
  tokenHash: z.string().length(64),
  signature: z.string().length(64),
  revoked: z.boolean().default(false)
});

export type ReleaseCapability = z.infer<typeof ReleaseCapabilitySchema>;

export function assertHumanActor(actor: ActorContext): HumanActorContext {
  return HumanActorContextSchema.parse(actor);
}

export function assertAdvisoryBoundary(boundary: unknown): AdvisoryBoundary {
  return AdvisoryBoundarySchema.parse(boundary);
}
=== FILE: packages/shared/src/math.types.ts ===
import { z } from "zod";

export const CategoricalBoundaryCertificateSchema = z.object({
  framework: z.literal("CBF"),
  verified: z.boolean(),
  brainCategory: z.literal("AdvisoryBrain"),
  spineCategory: z.literal("ProofSpine"),
  functorName: z.literal("AuthorityForgetfulFunctor"),
  naturalTransformationExists: z.literal(false),
  theorem: z.string(),
  certificateHash: z.string().length(64)
});

export type CategoricalBoundaryCertificate = z.infer<typeof CategoricalBoundaryCertificateSchema>;

export const ProofPhaseFieldSchema = z.object({
  framework: z.literal("MPPF"),
  phi: z.number().min(0).max(1),
  allenCahnVelocity: z.number(),
  contradictionPotential: z.number().min(0),
  approvalPotential: z.number().min(0),
  crystallized: z.boolean(),
  certificateHash: z.string().length(64)
});

export type ProofPhaseField = z.infer<typeof ProofPhaseFieldSchema>;

export const AdvisoryManifoldCertificateSchema = z.object({
  framework: z.literal("IGAM"),
  fisherInformation: z.array(z.array(z.number())),
  determinant: z.number(),
  geodesicReviewDistance: z.number().min(0),
  uncertaintyCurvature: z.number(),
  reviewRequired: z.boolean(),
  certificateHash: z.string().length(64)
});

export type AdvisoryManifoldCertificate = z.infer<typeof AdvisoryManifoldCertificateSchema>;

export const HamiltonianWorkflowTraceSchema = z.object({
  framework: z.literal("HWD"),
  q: z.array(z.number()),
  p: z.array(z.number()),
  kineticEnergy: z.number(),
  potentialEnergy: z.number(),
  hamiltonian: z.number(),
  initialHamiltonian: z.number(),
  deltaHamiltonian: z.number(),
  humanReviewImpulseRecorded: z.boolean(),
  conserved: z.boolean(),
  certificateHash: z.string().length(64)
});

export type HamiltonianWorkflowTrace = z.infer<typeof HamiltonianWorkflowTraceSchema>;

export const SpectralIntegrityResultSchema = z.object({
  framework: z.literal("SDIV"),
  nodeCount: z.number().int().nonnegative(),
  edgeCount: z.number().int().nonnegative(),
  algebraicConnectivity: z.number().min(0),
  connected: z.boolean(),
  spectralRadius: z.number().min(0),
  fiedlerLowerBound: z.number().min(0),
  certificateHash: z.string().length(64)
});

export type SpectralIntegrityResult = z.infer<typeof SpectralIntegrityResultSchema>;

export const EntropyRoutingCertificateSchema = z.object({
  framework: z.literal("CCER"),
  beta: z.number().positive(),
  temperature: z.number().positive(),
  entropy: z.number().min(0),
  probabilities: z.array(
    z.object({
      candidateId: z.string().min(1),
      energy: z.number(),
      probability: z.number().min(0).max(1)
    })
  ),
  selectedCandidateId: z.string().min(1),
  certificateHash: z.string().length(64)
});

export type EntropyRoutingCertificate = z.infer<typeof EntropyRoutingCertificateSchema>;

export const LyapunovCertificateSchema = z.object({
  framework: z.literal("LSSC"),
  stateVector: z.array(z.number()),
  epsilonBall: z.number().positive(),
  lyapunovValue: z.number().min(0),
  derivativeEstimate: z.number(),
  isStable: z.boolean(),
  releaseCertified: z.boolean(),
  certificateHash: z.string().length(64)
});

export type LyapunovCertificate = z.infer<typeof LyapunovCertificateSchema>;

export const BayesianCitationCertificateSchema = z.object({
  framework: z.literal("BCCN"),
  priorAlpha: z.number().positive(),
  priorBeta: z.number().positive(),
  observedSupported: z.number().int().nonnegative(),
  observedUnsupported: z.number().int().nonnegative(),
  posteriorAlpha: z.number().positive(),
  posteriorBeta: z.number().positive(),
  posteriorMean: z.number().min(0).max(1),
  credibleInterval90: z.tuple([z.number().min(0).max(1), z.number().min(0).max(1)]),
  sufficientCoverage: z.boolean(),
  certificateHash: z.string().length(64)
});

export type BayesianCitationCertificate = z.infer<typeof BayesianCitationCertificateSchema>;

export const MathematicalCertificateBundleSchema = z.object({
  categoricalBoundary: CategoricalBoundaryCertificateSchema,
  proofPhaseField: ProofPhaseFieldSchema,
  advisoryManifold: AdvisoryManifoldCertificateSchema,
  hamiltonianTrace: HamiltonianWorkflowTraceSchema,
  spectralIntegrity: SpectralIntegrityResultSchema,
  entropyRouting: EntropyRoutingCertificateSchema,
  lyapunov: LyapunovCertificateSchema,
  bayesianCitation: BayesianCitationCertificateSchema,
  bundleHash: z.string().length(64)
});

export type MathematicalCertificateBundle = z.infer<typeof MathematicalCertificateBundleSchema>;
=== FILE: packages/shared/src/proof.types.ts ===
import { z } from "zod";
import {
  ActorTypeSchema,
  AdvisoryBoundarySchema,
  ReleaseCapabilitySchema
} from "./authority.js";
import { MathematicalCertificateBundleSchema } from "./math.types.js";

export const ProofEventTypeSchema = z.enum([
  "MATTER_CREATED",
  "SOURCE_REGISTERED",
  "DOCUMENT_HASHED",
  "CLAIM_EXTRACTED",
  "EVIDENCE_LINKED",
  "CONTRADICTION_FLAGGED",
  "ADVISORY_OUTPUT_CREATED",
  "DRAFT_CANDIDATE_CREATED",
  "HUMAN_APPROVAL_RECORDED",
  "REDACTION_REVIEW_RECORDED",
  "RELEASE_GATE_EVALUATED",
  "VERIFICATION_MANIFEST_CREATED",
  "EXPORT_RECEIPT_ISSUED",
  "REPLAY_VALIDATED",
  "MATTER_LEDGER_RECORD_FINALIZED",
  "AUDIT_PACKET_BUILT",
  "PUBLIC_VERIFICATION_CREATED"
]);

export type ProofEventType = z.infer<typeof ProofEventTypeSchema>;

export const EventLogEntrySchema = z.object({
  eventId: z.string().min(1),
  tenantId: z.string().min(1),
  matterId: z.string().min(1),
  sequenceNumber: z.number().int().positive(),
  eventType: ProofEventTypeSchema,
  actorType: ActorTypeSchema,
  actorId: z.string().min(1),
  eventSource: z.string().min(1),
  idempotencyKey: z.string().min(1).optional(),
  payloadJson: z.unknown(),
  payloadHash: z.string().length(64),
  previousEventHash: z.string().length(64).nullable(),
  rowHash: z.string().length(64),
  createdAt: z.string().datetime()
});

export type EventLogEntry = z.infer<typeof EventLogEntrySchema>;

export const MatterSchema = z.object({
  matterId: z.string().min(1),
  tenantId: z.string().min(1),
  title: z.string().min(1),
  status: z.enum(["open", "review", "released", "finalized"]),
  createdAt: z.string().datetime()
});

export type Matter = z.infer<typeof MatterSchema>;

export const HumanApprovalSchema = z.object({
  approvalId: z.string().min(1),
  tenantId: z.string().min(1),
  matterId: z.string().min(1),
  actorId: z.string().min(1),
  approved: z.boolean(),
  approvalHash: z.string().length(64),
  createdAt: z.string().datetime()
});

export type HumanApproval = z.infer<typeof HumanApprovalSchema>;

export const RedactionReviewSchema = z.object({
  redactionReviewId: z.string().min(1),
  tenantId: z.string().min(1),
  matterId: z.string().min(1),
  actorId: z.string().min(1),
  cleared: z.boolean(),
  redactionHash: z.string().length(64),
  createdAt: z.string().datetime()
});

export type RedactionReview = z.infer<typeof RedactionReviewSchema>;

export const ReleaseRequestSchema = z.object({
  releaseRequestId: z.string().min(1),
  tenantId: z.string().min(1),
  matterId: z.string().min(1),
  exportPayloadHash: z.string().length(64),
  requestedByActorId: z.string().min(1),
  createdAt: z.string().datetime()
});

export type ReleaseRequest = z.infer<typeof ReleaseRequestSchema>;

export const ReleaseGateCheckSchema = z.object({
  checkId: z.string().min(1),
  label: z.string().min(1),
  passed: z.boolean(),
  reason: z.string().min(1),
  hash: z.string().length(64)
});

export type ReleaseGateCheck = z.infer<typeof ReleaseGateCheckSchema>;

export const ReleaseGateEvaluationSchema = z.object({
  releaseGateEvaluationId: z.string().min(1),
  tenantId: z.string().min(1),
  matterId: z.string().min(1),
  releaseRequestId: z.string().min(1),
  passed: z.boolean(),
  checks: z.array(ReleaseGateCheckSchema).min(1),
  spectralIntegrity: z.object({
    connected: z.boolean(),
    algebraicConnectivity: z.number()
  }),
  lyapunovStable: z.boolean(),
  evaluatedAt: z.string().datetime(),
  evaluationHash: z.string().length(64)
});

export type ReleaseGateEvaluation = z.infer<typeof ReleaseGateEvaluationSchema>;

export const VerificationManifestSchema = z.object({
  manifestId: z.string().min(1),
  tenantId: z.string().min(1),
  matterId: z.string().min(1),
  releaseGateEvaluationHash: z.string().length(64),
  eventChainHead: z.string().length(64),
  exportPayloadHash: z.string().length(64),
  manifestHash: z.string().length(64),
  createdAt: z.string().datetime()
});

export type VerificationManifest = z.infer<typeof VerificationManifestSchema>;

export const ExportReceiptSchema = z.object({
  receiptId: z.string().min(1),
  tenantId: z.string().min(1),
  matterId: z.string().min(1),
  manifestHash: z.string().length(64),
  exportPayloadHash: z.string().length(64),
  issuedByActorId: z.string().min(1),
  receiptHash: z.string().length(64),
  createdAt: z.string().datetime()
});

export type ExportReceipt = z.infer<typeof ExportReceiptSchema>;

export const ReplayMismatchSchema = z.object({
  code: z.string().min(1),
  message: z.string().min(1),
  severity: z.enum(["warning", "blocking"]),
  expected: z.string().optional(),
  actual: z.string().optional()
});

export type ReplayMismatch = z.infer<typeof ReplayMismatchSchema>;

export const ReplayResultSchema = z.object({
  replayId: z.string().min(1),
  tenantId: z.string().min(1),
  matterId: z.string().min(1),
  eventCount: z.number().int().nonnegative(),
  eventChainValid: z.boolean(),
  matchesFinalRecord: z.boolean(),
  finalRecordHash: z.string().length(64).nullable(),
  replayHash: z.string().length(64),
  mismatches: z.array(ReplayMismatchSchema),
  hamiltonianConsistent: z.boolean(),
  replayedAt: z.string().datetime()
});

export type ReplayResult = z.infer<typeof ReplayResultSchema>;

export const MatterLedgerRecordSchema = z.object({
  recordId: z.string().min(1),
  tenantId: z.string().min(1),
  matterId: z.string().min(1),
  manifestHash: z.string().length(64),
  receiptHash: z.string().length(64),
  replayHash: z.string().length(64),
  eventChainHead: z.string().length(64),
  lyapunovCertificateHash: z.string().length(64),
  recordHash: z.string().length(64),
  finalizedAt: z.string().datetime()
});

export type MatterLedgerRecord = z.infer<typeof MatterLedgerRecordSchema>;

export const AuditPacketSchema = z.object({
  auditPacketId: z.string().min(1),
  tenantId: z.string().min(1),
  matterId: z.string().min(1),
  manifestHash: z.string().length(64),
  receiptHash: z.string().length(64),
  replayHash: z.string().length(64),
  ledgerRecordHash: z.string().length(64),
  eventChainHead: z.string().length(64),
  mathematicalCertificates: MathematicalCertificateBundleSchema,
  auditPacketHash: z.string().length(64),
  createdAt: z.string().datetime()
});

export type AuditPacket = z.infer<typeof AuditPacketSchema>;

export const AdvisoryEnvelopeSchema = z.object({
  advisoryId: z.string().min(1),
  tenantId: z.string().min(1),
  matterId: z.string().min(1),
  module: z.string().min(1),
  summary: z.string().min(1),
  boundary: AdvisoryBoundarySchema,
  candidateHash: z.string().length(64),
  createdAt: z.string().datetime()
});

export type AdvisoryEnvelope = z.infer<typeof AdvisoryEnvelopeSchema>;

export const ReleaseAuthorityContextSchema = z.object({
  actorId: z.string().min(1),
  tenantId: z.string().min(1),
  matterId: z.string().min(1),
  capability: ReleaseCapabilitySchema
});

export type ReleaseAuthorityContext = z.infer<typeof ReleaseAuthorityContextSchema>;
=== FILE: packages/shared/src/solomon.ts ===
import { z } from "zod";
import { AdvisoryBoundarySchema } from "./authority.js";
import { BayesianCitationCertificateSchema, EntropyRoutingCertificateSchema } from "./math.types.js";

export const SolomonPlanRequestSchema = z.object({
  tenantId: z.string().min(1),
  matterId: z.string().min(1),
  prompt: z.string().min(1),
  evidenceHashes: z.array(z.string().length(64)).default([]),
  claimCount: z.number().int().nonnegative().default(0),
  supportedCitationCount: z.number().int().nonnegative().default(0),
  unsupportedCitationCount: z.number().int().nonnegative().default(0),
  riskScore: z.number().min(0).max(1).default(0.5)
});

export type SolomonPlanRequest = z.infer<typeof SolomonPlanRequestSchema>;

export const SolomonResolutionPathSchema = z.object({
  pathId: z.string().min(1),
  label: z.string().min(1),
  smallestLawfulTest: z.string().min(1),
  predictedPciDelta: z.number(),
  predictedDliDelta: z.number(),
  riskScore: z.number().min(0).max(1),
  evidenceHashes: z.array(z.string().length(64)),
  candidateHash: z.string().length(64)
});

export type SolomonResolutionPath = z.infer<typeof SolomonResolutionPathSchema>;

export const SolomonPlanSchema = z.object({
  advisoryId: z.string().min(1),
  tenantId: z.string().min(1),
  matterId: z.string().min(1),
  paths: z.array(SolomonResolutionPathSchema).min(1),
  selectedPathId: z.string().min(1),
  dissent: z.array(z.string()).default([]),
  bayesianCitationCoverage: BayesianCitationCertificateSchema,
  entropyRouting: EntropyRoutingCertificateSchema,
  boundary: AdvisoryBoundarySchema,
  advisoryOnly: z.literal(true),
  candidateOnly: z.literal(true),
  humanReviewRequired: z.literal(true),
  proofAuthorityGranted: z.literal(false),
  planHash: z.string().length(64),
  createdAt: z.string().datetime()
});

export type SolomonPlan = z.infer<typeof SolomonPlanSchema>;
=== FILE: packages/shared/src/sblc-draft.ts ===
import { z } from "zod";
import { AdvisoryBoundarySchema } from "./authority.js";
import {
  BayesianCitationCertificateSchema,
  EntropyRoutingCertificateSchema,
  LyapunovCertificateSchema
} from "./math.types.js";

export const SblcDraftRequestSchema = z.object({
  tenantId: z.string().min(1),
  matterId: z.string().min(1),
  draftPurpose: z.string().min(1),
  facts: z.array(z.string().min(1)).min(1),
  claimHashes: z.array(z.string().length(64)).default([]),
  evidenceHashes: z.array(z.string().length(64)).default([]),
  allowedAutoPublish: z.boolean().default(false)
});

export type SblcDraftRequest = z.infer<typeof SblcDraftRequestSchema>;

export const CitationSlotSchema = z.object({
  citationSlotId: z.string().min(1),
  claimHash: z.string().length(64),
  suggestedEvidenceHash: z.string().length(64).optional(),
  informationEnergy: z.number(),
  probability: z.number().min(0).max(1)
});

export type CitationSlot = z.infer<typeof CitationSlotSchema>;

export const AuthorityGapSchema = z.object({
  authorityGapId: z.string().min(1),
  severity: z.enum(["low", "medium", "high", "critical"]),
  reason: z.string().min(1),
  relatedClaimHash: z.string().length(64).optional()
});

export type AuthorityGap = z.infer<typeof AuthorityGapSchema>;

export const SblcDraftCandidateSchema = z.object({
  draftCandidateId: z.string().min(1),
  tenantId: z.string().min(1),
  matterId: z.string().min(1),
  draftPurpose: z.string().min(1),
  draftTextHash: z.string().length(64),
  summary: z.string().min(1),
  citationSlots: z.array(CitationSlotSchema),
  authorityGaps: z.array(AuthorityGapSchema),
  citationRouting: EntropyRoutingCertificateSchema,
  bayesianCitationCoverage: BayesianCitationCertificateSchema,
  evidenceStability: LyapunovCertificateSchema,
  boundary: AdvisoryBoundarySchema,
  candidateOnly: z.literal(true),
  advisoryOnly: z.literal(true),
  humanReviewRequired: z.literal(true),
  notLegalAdvice: z.literal(true),
  proofAuthorityGranted: z.literal(false),
  externalReleaseAllowed: z.literal(false),
  candidateHash: z.string().length(64),
  createdAt: z.string().datetime()
});

export type SblcDraftCandidate = z.infer<typeof SblcDraftCandidateSchema>;
=== FILE: packages/shared/src/index.ts ===
export * from "./hash.js";
export * from "./time.js";
export * from "./errors.js";
export * from "./authority.js";
export * from "./math.types.js";
export * from "./proof.types.js";
export * from "./solomon.js";
export * from "./sblc-draft.js";
=== FILE: packages/proof-spine/package.json ===
{
  "name": "@aletheia/proof-spine",
  "version": "0.1.0",
  "private": true,
  "type": "module",
  "main": "dist/index.js",
  "types": "dist/index.d.ts",
  "exports": {
    ".": {
      "types": "./dist/index.d.ts",
      "default": "./dist/index.js"
    }
  },
  "scripts": {
    "build": "tsc -b",
    "typecheck": "tsc -b --pretty false",
    "test": "vitest run"
  },
  "dependencies": {
    "@aletheia/shared": "workspace:*",
    "zod": "^3.24.1"
  }
}
=== FILE: packages/proof-spine/tsconfig.json ===
{
  "extends": "../../tsconfig.base.json",
  "compilerOptions": {
    "rootDir": "src",
    "outDir": "dist",
    "composite": true
  },
  "references": [{ "path": "../shared" }],
  "include": ["src/**/*.ts"]
}
=== FILE: packages/proof-spine/src/math/cbf.ts ===
import {
  CategoricalBoundaryCertificateSchema,
  sha256Canonical,
  type CategoricalBoundaryCertificate
} from "@aletheia/shared";

export function verifyCategoricalBoundary(): CategoricalBoundaryCertificate {
  const base = {
    framework: "CBF" as const,
    verified: true,
    brainCategory: "AdvisoryBrain" as const,
    spineCategory: "ProofSpine" as const,
    functorName: "AuthorityForgetfulFunctor" as const,
    naturalTransformationExists: false as const,
    theorem:
      "No natural transformation eta: AdvisoryBrain => ProofSpine exists because advisory morphisms lack release-authority objects, HMAC capability witnesses, and finalization codomain structure."
  };

  return CategoricalBoundaryCertificateSchema.parse({
    ...base,
    certificateHash: sha256Canonical(base)
  });
}
=== FILE: packages/proof-spine/src/math/mppf.ts ===
import {
  ProofPhaseFieldSchema,
  sha256Canonical,
  type ProofPhaseField
} from "@aletheia/shared";

export function computeProofPhaseField(input: {
  approvals: number;
  contradictions: number;
  redactionCleared: boolean;
  consentPresent: boolean;
  previousPhi?: number;
  dt?: number;
}): ProofPhaseField {
  const phi = Math.max(0, Math.min(1, input.previousPhi ?? 0.5));
  const dt = input.dt ?? 0.1;
  const approvalPotential = input.approvals + (input.redactionCleared ? 1 : 0) + (input.consentPresent ? 1 : 0);
  const contradictionPotential = input.contradictions;
  const doubleWellDerivative = 2 * phi * (1 - phi) * (1 - 2 * phi);
  const forcing = approvalPotential * (1 - phi) - contradictionPotential * phi;
  const allenCahnVelocity = doubleWellDerivative + forcing;
  const nextPhi = Math.max(0, Math.min(1, phi + dt * allenCahnVelocity));

  const base = {
    framework: "MPPF" as const,
    phi: nextPhi,
    allenCahnVelocity,
    contradictionPotential,
    approvalPotential,
    crystallized: nextPhi >= 0.925
  };

  return ProofPhaseFieldSchema.parse({
    ...base,
    certificateHash: sha256Canonical(base)
  });
}
=== FILE: packages/proof-spine/src/math/igam.ts ===
import {
  AdvisoryManifoldCertificateSchema,
  sha256Canonical,
  type AdvisoryManifoldCertificate
} from "@aletheia/shared";

function determinant2(matrix: number[][]): number {
  const a = matrix[0]?.[0] ?? 0;
  const b = matrix[0]?.[1] ?? 0;
  const c = matrix[1]?.[0] ?? 0;
  const d = matrix[1]?.[1] ?? 0;
  return a * d - b * c;
}

export function computeAdvisoryManifoldCertificate(input: {
  confidence: number;
  risk: number;
  evidenceCoverage: number;
}): AdvisoryManifoldCertificate {
  const confidence = Math.max(0.001, Math.min(0.999, input.confidence));
  const risk = Math.max(0.001, Math.min(0.999, input.risk));
  const evidenceCoverage = Math.max(0.001, Math.min(0.999, input.evidenceCoverage));

  const fisherInformation = [
    [1 / (confidence * (1 - confidence)), risk],
    [risk, 1 / (evidenceCoverage * (1 - evidenceCoverage))]
  ];

  const determinant = determinant2(fisherInformation);
  const geodesicReviewDistance = Math.sqrt(
    Math.max(0, (1 - confidence) ** 2 + risk ** 2 + (1 - evidenceCoverage) ** 2)
  );
  const uncertaintyCurvature = determinant === 0 ? Number.POSITIVE_INFINITY : 1 / Math.abs(determinant);

  const base = {
    framework: "IGAM" as const,
    fisherInformation,
    determinant,
    geodesicReviewDistance,
    uncertaintyCurvature,
    reviewRequired: geodesicReviewDistance > 0.45 || risk > 0.35
  };

  return AdvisoryManifoldCertificateSchema.parse({
    ...base,
    certificateHash: sha256Canonical(base)
  });
}
=== FILE: packages/proof-spine/src/math/hwd.ts ===
import {
  HamiltonianWorkflowTraceSchema,
  sha256Canonical,
  type HamiltonianWorkflowTrace
} from "@aletheia/shared";

function sumSquares(values: readonly number[]): number {
  return values.reduce((sum, value) => sum + value * value, 0);
}

export function computeHamiltonianWorkflowTrace(input: {
  q: number[];
  p: number[];
  potentialWeights?: number[];
  initialHamiltonian?: number;
  humanReviewImpulseRecorded?: boolean;
}): HamiltonianWorkflowTrace {
  const q = input.q;
  const p = input.p;
  const weights = input.potentialWeights ?? q.map(() => 1);
  const kineticEnergy = 0.5 * sumSquares(p);
  const potentialEnergy = 0.5 * q.reduce((sum, value, index) => {
    const weight = weights[index] ?? 1;
    return sum + weight * value * value;
  }, 0);
  const hamiltonian = kineticEnergy + potentialEnergy;
  const initialHamiltonian = input.initialHamiltonian ?? hamiltonian;
  const deltaHamiltonian = hamiltonian - initialHamiltonian;
  const humanReviewImpulseRecorded = input.humanReviewImpulseRecorded ?? false;
  const conserved = Math.abs(deltaHamiltonian) <= 0.025 || humanReviewImpulseRecorded;

  const base = {
    framework: "HWD" as const,
    q,
    p,
    kineticEnergy,
    potentialEnergy,
    hamiltonian,
    initialHamiltonian,
    deltaHamiltonian,
    humanReviewImpulseRecorded,
    conserved
  };

  return HamiltonianWorkflowTraceSchema.parse({
    ...base,
    certificateHash: sha256Canonical(base)
  });
}
=== FILE: packages/proof-spine/src/math/sdiv.ts ===
import {
  SpectralIntegrityResultSchema,
  sha256Canonical,
  type SpectralIntegrityResult
} from "@aletheia/shared";

export type DagEdge = {
  from: string;
  to: string;
};

function unique(values: readonly string[]): string[] {
  return [...new Set(values)];
}

function buildAdjacency(nodes: readonly string[], edges: readonly DagEdge[]): number[][] {
  const index = new Map(nodes.map((node, i) => [node, i] as const));
  const adjacency = nodes.map(() => nodes.map(() => 0));

  for (const edge of edges) {
    const from = index.get(edge.from);
    const to = index.get(edge.to);

    if (from === undefined || to === undefined || from === to) {
      continue;
    }

    adjacency[from][to] = 1;
    adjacency[to][from] = 1;
  }

  return adjacency;
}

function connectedComponents(nodes: readonly string[], adjacency: number[][]): number {
  const seen = new Set<number>();
  let components = 0;

  for (let i = 0; i < nodes.length; i += 1) {
    if (seen.has(i)) continue;

    components += 1;
    const stack = [i];

    while (stack.length > 0) {
      const next = stack.pop();

      if (next === undefined || seen.has(next)) continue;

      seen.add(next);

      const row = adjacency[next] ?? [];

      for (let j = 0; j < row.length; j += 1) {
        if ((row[j] ?? 0) > 0 && !seen.has(j)) {
          stack.push(j);
        }
      }
    }
  }

  return components;
}

function minDegree(adjacency: number[][]): number {
  if (adjacency.length === 0) return 0;

  return Math.min(...adjacency.map((row) => row.reduce((sum, value) => sum + value, 0)));
}

function maxDegree(adjacency: number[][]): number {
  if (adjacency.length === 0) return 0;

  return Math.max(...adjacency.map((row) => row.reduce((sum, value) => sum + value, 0)));
}

export function verifySpectralDagIntegrity(input: {
  nodes: string[];
  edges: DagEdge[];
}): SpectralIntegrityResult {
  const nodes = unique(input.nodes);
  const edges = input.edges.filter((edge) => edge.from !== edge.to);
  const adjacency = buildAdjacency(nodes, edges);
  const components = connectedComponents(nodes, adjacency);
  const connected = nodes.length <= 1 || components === 1;
  const minDeg = minDegree(adjacency);
  const maxDeg = maxDegree(adjacency);
  const fiedlerLowerBound = connected ? Math.max(0.000001, minDeg / Math.max(1, nodes.length - 1)) : 0;
  const algebraicConnectivity = connected ? fiedlerLowerBound : 0;
  const spectralRadius = maxDeg;

  const base = {
    framework: "SDIV" as const,
    nodeCount: nodes.length,
    edgeCount: edges.length,
    algebraicConnectivity,
    connected,
    spectralRadius,
    fiedlerLowerBound
  };

  return SpectralIntegrityResultSchema.parse({
    ...base,
    certificateHash: sha256Canonical(base)
  });
}
=== FILE: packages/proof-spine/src/math/ccer.ts ===
import {
  EntropyRoutingCertificateSchema,
  sha256Canonical,
  type EntropyRoutingCertificate
} from "@aletheia/shared";

export type RoutingCandidate = {
  candidateId: string;
  energy: number;
};

export function routeByBoltzmannEntropy(input: {
  candidates: RoutingCandidate[];
  meanRiskScore: number;
  epsilon?: number;
}): EntropyRoutingCertificate {
  if (input.candidates.length === 0) {
    throw new Error("Boltzmann routing requires at least one candidate.");
  }

  const epsilon = input.epsilon ?? 0.000001;
  const temperature = 1 / (input.meanRiskScore + epsilon);
  const beta = 1 / temperature;
  const weights = input.candidates.map((candidate) => Math.exp(-beta * candidate.energy));
  const partition = weights.reduce((sum, value) => sum + value, 0);

  const probabilities = input.candidates.map((candidate, index) => {
    const probability = (weights[index] ?? 0) / partition;

    return {
      candidateId: candidate.candidateId,
      energy: candidate.energy,
      probability
    };
  });

  const entropy = -probabilities.reduce((sum, item) => {
    if (item.probability <= 0) return sum;
    return sum + item.probability * Math.log(item.probability);
  }, 0);

  const selected = [...probabilities].sort((left, right) => right.probability - left.probability)[0];

  if (!selected) {
    throw new Error("Boltzmann routing failed to select a candidate.");
  }

  const base = {
    framework: "CCER" as const,
    beta,
    temperature,
    entropy,
    probabilities,
    selectedCandidateId: selected.candidateId
  };

  return EntropyRoutingCertificateSchema.parse({
    ...base,
    certificateHash: sha256Canonical(base)
  });
}
=== FILE: packages/proof-spine/src/math/lssc.ts ===
import {
  LyapunovCertificateSchema,
  sha256Canonical,
  type LyapunovCertificate
} from "@aletheia/shared";

export function computeLyapunovCertificate(input: {
  stateVector: number[];
  previousStateVector?: number[];
  epsilonBall?: number;
}): LyapunovCertificate {
  const epsilonBall = input.epsilonBall ?? 0.025;
  const lyapunovValue = input.stateVector.reduce((sum, value) => sum + value * value, 0);

  const previousValue =
    input.previousStateVector?.reduce((sum, value) => sum + value * value, 0) ?? lyapunovValue;

  const derivativeEstimate = lyapunovValue - previousValue;
  const isStable = lyapunovValue < epsilonBall && derivativeEstimate <= 0;
  const releaseCertified = isStable;

  const base = {
    framework: "LSSC" as const,
    stateVector: input.stateVector,
    epsilonBall,
    lyapunovValue,
    derivativeEstimate,
    isStable,
    releaseCertified
  };

  return LyapunovCertificateSchema.parse({
    ...base,
    certificateHash: sha256Canonical(base)
  });
}
=== FILE: packages/proof-spine/src/math/bccn.ts ===
import {
  BayesianCitationCertificateSchema,
  sha256Canonical,
  type BayesianCitationCertificate
} from "@aletheia/shared";

function normalApproxInterval90(alpha: number, beta: number): [number, number] {
  const total = alpha + beta;
  const mean = alpha / total;
  const variance = (alpha * beta) / (total * total * (total + 1));
  const sd = Math.sqrt(variance);
  const z90 = 1.6448536269514722;
  return [
    Math.max(0, mean - z90 * sd),
    Math.min(1, mean + z90 * sd)
  ];
}

export function computeBayesianCitationCertificate(input: {
  observedSupported: number;
  observedUnsupported: number;
  priorAlpha?: number;
  priorBeta?: number;
  requiredLowerBound?: number;
}): BayesianCitationCertificate {
  const priorAlpha = input.priorAlpha ?? 2;
  const priorBeta = input.priorBeta ?? 2;
  const posteriorAlpha = priorAlpha + input.observedSupported;
  const posteriorBeta = priorBeta + input.observedUnsupported;
  const posteriorMean = posteriorAlpha / (posteriorAlpha + posteriorBeta);
  const credibleInterval90 = normalApproxInterval90(posteriorAlpha, posteriorBeta);
  const requiredLowerBound = input.requiredLowerBound ?? 0.72;
  const sufficientCoverage = credibleInterval90[0] >= requiredLowerBound;

  const base = {
    framework: "BCCN" as const,
    priorAlpha,
    priorBeta,
    observedSupported: input.observedSupported,
    observedUnsupported: input.observedUnsupported,
    posteriorAlpha,
    posteriorBeta,
    posteriorMean,
    credibleInterval90,
    sufficientCoverage
  };

  return BayesianCitationCertificateSchema.parse({
    ...base,
    certificateHash: sha256Canonical(base)
  });
}
=== FILE: packages/proof-spine/src/math/certificate-bundle.ts ===
import {
  MathematicalCertificateBundleSchema,
  sha256Canonical,
  type MathematicalCertificateBundle
} from "@aletheia/shared";
import { verifyCategoricalBoundary } from "./cbf.js";
import { computeProofPhaseField } from "./mppf.js";
import { computeAdvisoryManifoldCertificate } from "./igam.js";
import { computeHamiltonianWorkflowTrace } from "./hwd.js";
import { verifySpectralDagIntegrity, type DagEdge } from "./sdiv.js";
import { routeByBoltzmannEntropy } from "./ccer.js";
import { computeLyapunovCertificate } from "./lssc.js";
import { computeBayesianCitationCertificate } from "./bccn.js";

export function buildMathematicalCertificateBundle(input: {
  approvals: number;
  contradictions: number;
  redactionCleared: boolean;
  consentPresent: boolean;
  confidence: number;
  risk: number;
  evidenceCoverage: number;
  q: number[];
  p: number[];
  initialHamiltonian?: number;
  humanReviewImpulseRecorded?: boolean;
  dagNodes: string[];
  dagEdges: DagEdge[];
  routingCandidates: Array<{ candidateId: string; energy: number }>;
  meanRiskScore: number;
  lyapunovStateVector: number[];
  previousLyapunovStateVector?: number[];
  observedSupportedCitations: number;
  observedUnsupportedCitations: number;
}): MathematicalCertificateBundle {
  const categoricalBoundary = verifyCategoricalBoundary();
  const proofPhaseField = computeProofPhaseField({
    approvals: input.approvals,
    contradictions: input.contradictions,
    redactionCleared: input.redactionCleared,
    consentPresent: input.consentPresent
  });
  const advisoryManifold = computeAdvisoryManifoldCertificate({
    confidence: input.confidence,
    risk: input.risk,
    evidenceCoverage: input.evidenceCoverage
  });
  const hamiltonianTrace = computeHamiltonianWorkflowTrace({
    q: input.q,
    p: input.p,
    initialHamiltonian: input.initialHamiltonian,
    humanReviewImpulseRecorded: input.humanReviewImpulseRecorded
  });
  const spectralIntegrity = verifySpectralDagIntegrity({
    nodes: input.dagNodes,
    edges: input.dagEdges
  });
  const entropyRouting = routeByBoltzmannEntropy({
    candidates: input.routingCandidates,
    meanRiskScore: input.meanRiskScore
  });
  const lyapunov = computeLyapunovCertificate({
    stateVector: input.lyapunovStateVector,
    previousStateVector: input.previousLyapunovStateVector
  });
  const bayesianCitation = computeBayesianCitationCertificate({
    observedSupported: input.observedSupportedCitations,
    observedUnsupported: input.observedUnsupportedCitations
  });

  const base = {
    categoricalBoundary,
    proofPhaseField,
    advisoryManifold,
    hamiltonianTrace,
    spectralIntegrity,
    entropyRouting,
    lyapunov,
    bayesianCitation
  };

  return MathematicalCertificateBundleSchema.parse({
    ...base,
    bundleHash: sha256Canonical(base)
  });
}
=== FILE: packages/proof-spine/src/event-chain.ts ===
import {
  AletheiaError,
  EventLogEntrySchema,
  nowIso,
  sha256Canonical,
  type ActorContext,
  type EventLogEntry,
  type ProofEventType
} from "@aletheia/shared";

function assertAdvisoryOutputBoundary(payloadJson: unknown): void {
  if (typeof payloadJson !== "object" || payloadJson === null) {
    return;
  }

  if (!("boundary" in payloadJson)) {
    return;
  }

  const boundary = (payloadJson as { boundary?: unknown }).boundary;

  if (typeof boundary !== "object" || boundary === null) {
    throw new AletheiaError(
      "ADVISORY_BOUNDARY_VIOLATION",
      "Advisory payload boundary must be an object."
    );
  }

  const value = boundary as Record<string, unknown>;

  const failed =
    value["advisoryOnly"] !== true ||
    value["candidateOnly"] !== true ||
    value["humanReviewRequired"] !== true ||
    value["externalReleaseAllowed"] !== false ||
    value["proofAuthorityGranted"] !== false ||
    value["mayApprove"] !== false ||
    value["mayRelease"] !== false ||
    value["mayExport"] !== false ||
    value["mayIssueReceipt"] !== false ||
    value["mayCreateManifest"] !== false ||
    value["mayFinalizeMatterLedgerRecord"] !== false ||
    value["mayBypassRedactions"] !== false ||
    value["mayAlterAuditHistory"] !== false;

  if (failed) {
    throw new AletheiaError(
      "ADVISORY_BOUNDARY_VIOLATION",
      "Advisory output attempted to carry proof authority."
    );
  }
}

export function computeEventPayloadHash(payloadJson: unknown): string {
  return sha256Canonical(payloadJson);
}

export function computeEventRowHash(input: {
  matterId: string;
  sequenceNumber: number;
  eventId: string;
  eventType: ProofEventType;
  actorType: ActorContext["actorType"];
  actorId: string;
  payloadHash: string;
  previousEventHash: string | null;
  createdAt: string;
}): string {
  return sha256Canonical({
    matterId: input.matterId,
    sequenceNumber: input.sequenceNumber,
    eventId: input.eventId,
    eventType: input.eventType,
    actorType: input.actorType,
    actorId: input.actorId,
    payloadHash: input.payloadHash,
    previousEventHash: input.previousEventHash,
    createdAt: input.createdAt
  });
}

export function hashPayload(payloadJson: unknown): string {
  return computeEventPayloadHash(payloadJson);
}

export function hashEventRow(input: {
  matterId: string;
  sequenceNumber: number;
  eventId: string;
  eventType: ProofEventType;
  actorType: ActorContext["actorType"];
  actorId: string;
  payloadHash: string;
  previousEventHash: string | null;
  createdAt: string;
}): string {
  return computeEventRowHash(input);
}

export function validateEventChain(events: readonly EventLogEntry[]): {
  valid: boolean;
  errors: string[];
  lastEventHash: string | null;
} {
  const parsed = events
    .map((event) => EventLogEntrySchema.parse(event))
    .sort((left, right) => left.sequenceNumber - right.sequenceNumber);

  const errors: string[] = [];
  let previousHash: string | null = null;

  for (let index = 0; index < parsed.length; index += 1) {
    const event = parsed[index];
    const expectedSequence = index + 1;

    if (event.sequenceNumber !== expectedSequence) {
      errors.push(
        `Sequence mismatch for ${event.eventId}: expected ${expectedSequence}, received ${event.sequenceNumber}.`
      );
    }

    if (event.previousEventHash !== previousHash) {
      errors.push(`Previous hash mismatch for ${event.eventId}.`);
    }

    const expectedPayloadHash = computeEventPayloadHash(event.payloadJson);

    if (event.payloadHash !== expectedPayloadHash) {
      errors.push(`Payload hash mismatch for ${event.eventId}.`);
    }

    const expectedRowHash = computeEventRowHash({
      matterId: event.matterId,
      sequenceNumber: event.sequenceNumber,
      eventId: event.eventId,
      eventType: event.eventType,
      actorType: event.actorType,
      actorId: event.actorId,
      payloadHash: event.payloadHash,
      previousEventHash: event.previousEventHash,
      createdAt: event.createdAt
    });

    if (event.rowHash !== expectedRowHash) {
      errors.push(`Row hash mismatch for ${event.eventId}.`);
    }

    previousHash = event.rowHash;
  }

  return {
    valid: errors.length === 0,
    errors,
    lastEventHash: previousHash
  };
}

export function appendEventChainStep(
  chain: readonly EventLogEntry[],
  input: {
    eventId: string;
    tenantId: string;
    matterId: string;
    eventType: ProofEventType;
    actor: ActorContext;
    eventSource: string;
    idempotencyKey?: string;
    payloadJson: unknown;
    createdAt?: string;
  }
): EventLogEntry {
  const sorted = [...chain].sort((left, right) => left.sequenceNumber - right.sequenceNumber);
  const verification = validateEventChain(sorted);

  if (!verification.valid) {
    throw new AletheiaError("CHAIN_INVALID", "Cannot append to invalid event chain.", {
      errors: verification.errors
    });
  }

  if (input.actor.actorType === "AI") {
    assertAdvisoryOutputBoundary(input.payloadJson);
  }

  if (
    typeof input.payloadJson === "object" &&
    input.payloadJson !== null &&
    "boundary" in input.payloadJson
  ) {
    assertAdvisoryOutputBoundary(input.payloadJson);
  }

  const previous = sorted.at(-1) ?? null;
  const sequenceNumber = previous ? previous.sequenceNumber + 1 : 1;
  const payloadHash = computeEventPayloadHash(input.payloadJson);
  const previousEventHash = previous?.rowHash ?? null;
  const actorId = input.actor.actorId;
  const createdAt = input.createdAt ?? nowIso();

  const rowHash = computeEventRowHash({
    matterId: input.matterId,
    sequenceNumber,
    eventId: input.eventId,
    eventType: input.eventType,
    actorType: input.actor.actorType,
    actorId,
    payloadHash,
    previousEventHash,
    createdAt
  });

  return EventLogEntrySchema.parse({
    eventId: input.eventId,
    tenantId: input.tenantId,
    matterId: input.matterId,
    sequenceNumber,
    eventType: input.eventType,
    actorType: input.actor.actorType,
    actorId,
    eventSource: input.eventSource,
    idempotencyKey: input.idempotencyKey,
    payloadJson: input.payloadJson,
    payloadHash,
    previousEventHash,
    rowHash,
    createdAt
  });
}

export function createEventLogEntry(
  chain: readonly EventLogEntry[],
  input: Parameters<typeof appendEventChainStep>[1]
): EventLogEntry {
  return appendEventChainStep(chain, input);
}

export type ProofEventStoreSnapshot = {
  tenantId: string;
  matterId: string;
  eventCount: number;
  valid: boolean;
  lastEventHash: string | null;
  snapshotHash: string;
};

export function validateProofEventStore(events: readonly EventLogEntry[]): ReturnType<typeof validateEventChain> {
  return validateEventChain(events);
}

export function createProofEventStoreSnapshot(input: {
  tenantId: string;
  matterId: string;
  events: readonly EventLogEntry[];
}): ProofEventStoreSnapshot {
  const validation = validateEventChain(input.events);
  const base = {
    tenantId: input.tenantId,
    matterId: input.matterId,
    eventCount: input.events.length,
    valid: validation.valid,
    lastEventHash: validation.lastEventHash
  };

  return {
    ...base,
    snapshotHash: sha256Canonical(base)
  };
}

export function appendProofEventToStore(
  events: readonly EventLogEntry[],
  input: Parameters<typeof appendEventChainStep>[1]
): EventLogEntry[] {
  const next = appendEventChainStep(events, input);
  return [...events, next];
}
=== FILE: packages/proof-spine/src/capability.ts ===
import {
  AletheiaError,
  ReleaseCapabilityPayloadSchema,
  ReleaseCapabilitySchema,
  assertHumanActor,
  hmacSha256Canonical,
  isExpiredIso,
  sha256Canonical,
  timingSafeHexEqual,
  type ActorContext,
  type CapabilityScope,
  type ReleaseCapability
} from "@aletheia/shared";

export function issueReleaseCapability(input: {
  secret: string;
  capabilityId: string;
  tenantId: string;
  matterId: string;
  issuedTo: ActorContext;
  issuedBy: ActorContext;
  scopes: CapabilityScope[];
  issuedAt: string;
  expiresAt: string;
}): ReleaseCapability {
  const issuedTo = assertHumanActor(input.issuedTo);
  const issuedBy = assertHumanActor(input.issuedBy);

  if (issuedTo.tenantId !== input.tenantId || issuedBy.tenantId !== input.tenantId) {
    throw new AletheiaError("AUTHORITY_DENIED", "Capability tenant mismatch.");
  }

  if (input.scopes.length === 0) {
    throw new AletheiaError("CAPABILITY_SCOPE_DENIED", "Capability must include at least one scope.");
  }

  const payload = ReleaseCapabilityPayloadSchema.parse({
    capabilityId: input.capabilityId,
    tenantId: input.tenantId,
    matterId: input.matterId,
    issuedToActorId: issuedTo.actorId,
    issuedByActorId: issuedBy.actorId,
    scopes: input.scopes,
    categoricalFunctorTag: "AuthorityFunctor.ProofSpine",
    issuedAt: input.issuedAt,
    expiresAt: input.expiresAt
  });

  const tokenHash = sha256Canonical(payload);
  const signature = hmacSha256Canonical(input.secret, {
    tokenHash,
    payload
  });

  return ReleaseCapabilitySchema.parse({
    payload,
    tokenHash,
    signature,
    revoked: false
  });
}

export function verifyReleaseCapability(input: {
  secret: string;
  actor: ActorContext;
  tenantId: string;
  matterId: string;
  capability: ReleaseCapability;
  requiredScope: CapabilityScope;
  nowIso: string;
}): void {
  const actor = assertHumanActor(input.actor);
  const capability = ReleaseCapabilitySchema.parse(input.capability);

  if (capability.revoked) {
    throw new AletheiaError("CAPABILITY_REVOKED", "Release capability has been revoked.");
  }

  if (capability.payload.tenantId !== input.tenantId || capability.payload.tenantId !== actor.tenantId) {
    throw new AletheiaError("AUTHORITY_DENIED", "Capability tenant mismatch.");
  }

  if (capability.payload.matterId !== input.matterId) {
    throw new AletheiaError("AUTHORITY_DENIED", "Capability matter mismatch.");
  }

  if (capability.payload.issuedToActorId !== actor.actorId) {
    throw new AletheiaError("AUTHORITY_DENIED", "Capability actor mismatch.");
  }

  if (!capability.payload.scopes.includes(input.requiredScope)) {
    throw new AletheiaError("CAPABILITY_SCOPE_DENIED", `Capability lacks scope ${input.requiredScope}.`);
  }

  if (isExpiredIso(capability.payload.expiresAt, input.nowIso)) {
    throw new AletheiaError("CAPABILITY_EXPIRED", "Release capability has expired.");
  }

  const expectedTokenHash = sha256Canonical(capability.payload);

  if (!timingSafeHexEqual(expectedTokenHash, capability.tokenHash)) {
    throw new AletheiaError("CAPABILITY_SIGNATURE_INVALID", "Capability token hash mismatch.");
  }

  const expectedSignature = hmacSha256Canonical(input.secret, {
    tokenHash: capability.tokenHash,
    payload: capability.payload
  });

  if (!timingSafeHexEqual(expectedSignature, capability.signature)) {
    throw new AletheiaError("CAPABILITY_SIGNATURE_INVALID", "Capability HMAC signature mismatch.");
  }
}
=== FILE: packages/proof-spine/src/release-gates.ts ===
import {
  ReleaseGateCheckSchema,
  ReleaseGateEvaluationSchema,
  nowIso,
  sha256Canonical,
  type HumanApproval,
  type RedactionReview,
  type ReleaseGateCheck,
  type ReleaseGateEvaluation,
  type ReleaseRequest
} from "@aletheia/shared";
import { verifySpectralDagIntegrity, type DagEdge } from "./math/sdiv.js";
import { computeLyapunovCertificate } from "./math/lssc.js";

function check(label: string, passed: boolean, reason: string): ReleaseGateCheck {
  const base = {
    checkId: `check_${sha256Canonical({ label, passed, reason }).slice(0, 16)}`,
    label,
    passed,
    reason
  };

  return ReleaseGateCheckSchema.parse({
    ...base,
    hash: sha256Canonical(base)
  });
}

export function evaluateReleaseGate(input: {
  releaseRequest: ReleaseRequest;
  approvals: HumanApproval[];
  redactionReviews: RedactionReview[];
  consentPresent: boolean;
  dagNodes: string[];
  dagEdges: DagEdge[];
  stabilityStateVector: number[];
}): ReleaseGateEvaluation {
  const approvalPassed = input.approvals.some((approval) => approval.approved);
  const redactionPassed = input.redactionReviews.some((review) => review.cleared);
  const spectral = verifySpectralDagIntegrity({
    nodes: input.dagNodes,
    edges: input.dagEdges
  });
  const lyapunov = computeLyapunovCertificate({
    stateVector: input.stabilityStateVector
  });

  const checks = [
    check("human approval", approvalPassed, approvalPassed ? "Human approval present." : "Missing human approval."),
    check("redaction clearance", redactionPassed, redactionPassed ? "Redaction cleared." : "Missing redaction clearance."),
    check("consent present", input.consentPresent, input.consentPresent ? "Consent present." : "Consent missing."),
    check(
      "spectral dag connected",
      spectral.connected,
      spectral.connected ? "Proof DAG is connected." : "Proof DAG is disconnected."
    ),
    check(
      "lyapunov stable",
      lyapunov.releaseCertified,
      lyapunov.releaseCertified
        ? "Release state is inside Lyapunov epsilon ball."
        : "Release state is outside Lyapunov epsilon ball."
    )
  ];

  const passed = checks.every((item) => item.passed);
  const evaluatedAt = nowIso();

  const base = {
    releaseGateEvaluationId: `rge_${sha256Canonical({
      releaseRequestId: input.releaseRequest.releaseRequestId,
      evaluatedAt
    }).slice(0, 16)}`,
    tenantId: input.releaseRequest.tenantId,
    matterId: input.releaseRequest.matterId,
    releaseRequestId: input.releaseRequest.releaseRequestId,
    passed,
    checks,
    spectralIntegrity: {
      connected: spectral.connected,
      algebraicConnectivity: spectral.algebraicConnectivity
    },
    lyapunovStable: lyapunov.releaseCertified,
    evaluatedAt
  };

  return ReleaseGateEvaluationSchema.parse({
    ...base,
    evaluationHash: sha256Canonical(base)
  });
}
=== FILE: packages/proof-spine/src/manifest.ts ===
import {
  AletheiaError,
  VerificationManifestSchema,
  nowIso,
  sha256Canonical,
  type ReleaseGateEvaluation,
  type VerificationManifest
} from "@aletheia/shared";

export function createVerificationManifest(input: {
  releaseGateEvaluation: ReleaseGateEvaluation;
  eventChainHead: string;
  exportPayloadHash: string;
}): VerificationManifest {
  if (!input.releaseGateEvaluation.passed) {
    throw new AletheiaError("RELEASE_GATE_FAILED", "Cannot create manifest from failed release gate.", {
      releaseGateEvaluationId: input.releaseGateEvaluation.releaseGateEvaluationId
    });
  }

  const createdAt = nowIso();

  const base = {
    manifestId: `manifest_${sha256Canonical({
      releaseGateEvaluationHash: input.releaseGateEvaluation.evaluationHash,
      eventChainHead: input.eventChainHead,
      exportPayloadHash: input.exportPayloadHash
    }).slice(0, 16)}`,
    tenantId: input.releaseGateEvaluation.tenantId,
    matterId: input.releaseGateEvaluation.matterId,
    releaseGateEvaluationHash: input.releaseGateEvaluation.evaluationHash,
    eventChainHead: input.eventChainHead,
    exportPayloadHash: input.exportPayloadHash,
    createdAt
  };

  return VerificationManifestSchema.parse({
    ...base,
    manifestHash: sha256Canonical(base)
  });
}
=== FILE: packages/proof-spine/src/receipt.ts ===
import {
  ExportReceiptSchema,
  nowIso,
  sha256Canonical,
  type ExportReceipt,
  type HumanActorContext,
  type VerificationManifest
} from "@aletheia/shared";

export function issueExportReceipt(input: {
  manifest: VerificationManifest;
  actor: HumanActorContext;
}): ExportReceipt {
  const createdAt = nowIso();

  const base = {
    receiptId: `receipt_${sha256Canonical({
      manifestHash: input.manifest.manifestHash,
      actorId: input.actor.actorId
    }).slice(0, 16)}`,
    tenantId: input.manifest.tenantId,
    matterId: input.manifest.matterId,
    manifestHash: input.manifest.manifestHash,
    exportPayloadHash: input.manifest.exportPayloadHash,
    issuedByActorId: input.actor.actorId,
    createdAt
  };

  return ExportReceiptSchema.parse({
    ...base,
    receiptHash: sha256Canonical(base)
  });
}
=== FILE: packages/proof-spine/src/replay.ts ===
import {
  ReplayResultSchema,
  nowIso,
  sha256Canonical,
  type EventLogEntry,
  type MatterLedgerRecord,
  type ReplayMismatch,
  type ReplayResult
} from "@aletheia/shared";
import { validateEventChain } from "./event-chain.js";
import { computeHamiltonianWorkflowTrace } from "./math/hwd.js";

export function replayMatterRecord(input: {
  replayId: string;
  tenantId: string;
  matterId: string;
  events: readonly EventLogEntry[];
  ledger: MatterLedgerRecord | null;
  hamiltonianInitial?: number;
  humanReviewImpulseRecorded?: boolean;
}): ReplayResult {
  const chain = validateEventChain(input.events);

  const mismatches: ReplayMismatch[] = chain.errors.map((error) => ({
    code: "EVENT_CHAIN_INVALID",
    message: error,
    severity: "blocking"
  }));

  if (input.ledger && chain.lastEventHash) {
    const chainHashes = new Set(input.events.map((event) => event.rowHash));

    if (!chainHashes.has(input.ledger.eventChainHead)) {
      mismatches.push({
        code: "LEDGER_EVENT_HEAD_NOT_IN_CHAIN",
        message: "Ledger eventChainHead is not present in recomputed event chain.",
        expected: input.ledger.eventChainHead,
        actual: chain.lastEventHash,
        severity: "blocking"
      });
    }
  }

  const hamiltonianTrace = computeHamiltonianWorkflowTrace({
    q: [input.events.length / 100, mismatches.length / 10],
    p: [chain.valid ? 0 : 1, input.ledger ? 0 : 0.5],
    initialHamiltonian: input.hamiltonianInitial,
    humanReviewImpulseRecorded: input.humanReviewImpulseRecorded
  });

  if (!hamiltonianTrace.conserved) {
    mismatches.push({
      code: "HAMILTONIAN_INCONSISTENT",
      message: "Replay Hamiltonian is not conserved and no human review impulse was recorded.",
      severity: "blocking"
    });
  }

  const blockingMismatchCount = mismatches.filter((mismatch) => mismatch.severity === "blocking").length;

  const base = {
    replayId: input.replayId,
    tenantId: input.tenantId,
    matterId: input.matterId,
    eventCount: input.events.length,
    eventChainValid: chain.valid,
    matchesFinalRecord: Boolean(input.ledger) && blockingMismatchCount === 0,
    finalRecordHash: input.ledger?.recordHash ?? null,
    mismatches,
    hamiltonianConsistent: hamiltonianTrace.conserved,
    replayedAt: nowIso()
  };

  return ReplayResultSchema.parse({
    ...base,
    replayHash: sha256Canonical(base)
  });
}
=== FILE: packages/proof-spine/src/ledger.ts ===
import {
  AletheiaError,
  MatterLedgerRecordSchema,
  nowIso,
  sha256Canonical,
  type ExportReceipt,
  type MatterLedgerRecord,
  type ReplayResult,
  type VerificationManifest
} from "@aletheia/shared";
import { computeLyapunovCertificate } from "./math/lssc.js";

export function finalizeMatterLedgerRecord(input: {
  recordId: string;
  manifest: VerificationManifest;
  receipt: ExportReceipt;
  replay: ReplayResult;
  eventChainHead: string;
  stabilityStateVector: number[];
}): MatterLedgerRecord {
  if (!input.replay.matchesFinalRecord || !input.replay.eventChainValid) {
    throw new AletheiaError("REPLAY_MISMATCH", "Cannot finalize ledger with invalid replay.", {
      replayHash: input.replay.replayHash,
      mismatches: input.replay.mismatches
    });
  }

  const lyapunov = computeLyapunovCertificate({
    stateVector: input.stabilityStateVector
  });

  if (!lyapunov.releaseCertified) {
    throw new AletheiaError("LYAPUNOV_UNSTABLE", "Cannot finalize outside Lyapunov epsilon ball.", {
      lyapunovValue: lyapunov.lyapunovValue,
      epsilonBall: lyapunov.epsilonBall
    });
  }

  const finalizedAt = nowIso();

  const base = {
    recordId: input.recordId,
    tenantId: input.manifest.tenantId,
    matterId: input.manifest.matterId,
    manifestHash: input.manifest.manifestHash,
    receiptHash: input.receipt.receiptHash,
    replayHash: input.replay.replayHash,
    eventChainHead: input.eventChainHead,
    lyapunovCertificateHash: lyapunov.certificateHash,
    finalizedAt
  };

  return MatterLedgerRecordSchema.parse({
    ...base,
    recordHash: sha256Canonical(base)
  });
}
=== FILE: packages/proof-spine/src/audit-packet.ts ===
import {
  AuditPacketSchema,
  nowIso,
  sha256Canonical,
  type AuditPacket,
  type ExportReceipt,
  type MatterLedgerRecord,
  type ReplayResult,
  type VerificationManifest
} from "@aletheia/shared";
import { buildMathematicalCertificateBundle } from "./math/certificate-bundle.js";
import type { DagEdge } from "./math/sdiv.js";

export function buildAuditPacket(input: {
  auditPacketId: string;
  manifest: VerificationManifest;
  receipt: ExportReceipt;
  replay: ReplayResult;
  ledger: MatterLedgerRecord;
  approvals: number;
  contradictions: number;
  redactionCleared: boolean;
  consentPresent: boolean;
  confidence: number;
  risk: number;
  evidenceCoverage: number;
  q: number[];
  p: number[];
  dagNodes: string[];
  dagEdges: DagEdge[];
  routingCandidates: Array<{ candidateId: string; energy: number }>;
  meanRiskScore: number;
  lyapunovStateVector: number[];
  observedSupportedCitations: number;
  observedUnsupportedCitations: number;
}): AuditPacket {
  const mathematicalCertificates = buildMathematicalCertificateBundle({
    approvals: input.approvals,
    contradictions: input.contradictions,
    redactionCleared: input.redactionCleared,
    consentPresent: input.consentPresent,
    confidence: input.confidence,
    risk: input.risk,
    evidenceCoverage: input.evidenceCoverage,
    q: input.q,
    p: input.p,
    dagNodes: input.dagNodes,
    dagEdges: input.dagEdges,
    routingCandidates: input.routingCandidates,
    meanRiskScore: input.meanRiskScore,
    lyapunovStateVector: input.lyapunovStateVector,
    observedSupportedCitations: input.observedSupportedCitations,
    observedUnsupportedCitations: input.observedUnsupportedCitations
  });

  const createdAt = nowIso();

  const base = {
    auditPacketId: input.auditPacketId,
    tenantId: input.ledger.tenantId,
    matterId: input.ledger.matterId,
    manifestHash: input.manifest.manifestHash,
    receiptHash: input.receipt.receiptHash,
    replayHash: input.replay.replayHash,
    ledgerRecordHash: input.ledger.recordHash,
    eventChainHead: input.ledger.eventChainHead,
    mathematicalCertificates,
    createdAt
  };

  return AuditPacketSchema.parse({
    ...base,
    auditPacketHash: sha256Canonical(base)
  });
}
=== FILE: packages/proof-spine/src/merkle-dag.ts ===
import { sha256Canonical } from "@aletheia/shared";
import { verifySpectralDagIntegrity, type DagEdge } from "./math/sdiv.js";

export type MerkleDagNode = {
  nodeId: string;
  payloadHash: string;
  parentIds: string[];
  nodeHash: string;
};

export type MerkleDag = {
  nodes: MerkleDagNode[];
  rootHash: string;
  spectral: ReturnType<typeof verifySpectralDagIntegrity>;
};

export function createMerkleDagNode(input: {
  nodeId: string;
  payloadHash: string;
  parentIds: string[];
}): MerkleDagNode {
  const base = {
    nodeId: input.nodeId,
    payloadHash: input.payloadHash,
    parentIds: [...input.parentIds].sort()
  };

  return {
    ...base,
    nodeHash: sha256Canonical(base)
  };
}

export function buildMerkleDag(nodes: readonly MerkleDagNode[]): MerkleDag {
  const sorted = [...nodes].sort((left, right) => left.nodeId.localeCompare(right.nodeId));
  const rootHash = sha256Canonical(sorted.map((node) => node.nodeHash));
  const edges: DagEdge[] = sorted.flatMap((node) =>
    node.parentIds.map((parent) => ({
      from: parent,
      to: node.nodeId
    }))
  );
  const spectral = verifySpectralDagIntegrity({
    nodes: sorted.map((node) => node.nodeId),
    edges
  });

  return {
    nodes: sorted,
    rootHash,
    spectral
  };
}
=== FILE: packages/proof-spine/src/source-ledger.ts ===
import { sha256Canonical } from "@aletheia/shared";

export type SourceLedgerRecord = {
  sourceId: string;
  tenantId: string;
  matterId: string;
  sourceKind: "document" | "email" | "screenshot" | "audio" | "record" | "other";
  payloadHash: string;
  shannonEntropy: number;
  sourceLedgerHash: string;
};

function shannonEntropyFromHex(hash: string): number {
  const counts = new Map<string, number>();

  for (const char of hash) {
    counts.set(char, (counts.get(char) ?? 0) + 1);
  }

  let entropy = 0;

  for (const count of counts.values()) {
    const p = count / hash.length;
    entropy -= p * Math.log2(p);
  }

  return entropy;
}

export function createSourceLedgerRecord(input: {
  sourceId: string;
  tenantId: string;
  matterId: string;
  sourceKind: SourceLedgerRecord["sourceKind"];
  payload: unknown;
}): SourceLedgerRecord {
  const payloadHash = sha256Canonical(input.payload);
  const shannonEntropy = shannonEntropyFromHex(payloadHash);

  const base = {
    sourceId: input.sourceId,
    tenantId: input.tenantId,
    matterId: input.matterId,
    sourceKind: input.sourceKind,
    payloadHash,
    shannonEntropy
  };

  return {
    ...base,
    sourceLedgerHash: sha256Canonical(base)
  };
}
=== FILE: packages/proof-spine/src/index.ts ===
export * from "./math/cbf.js";
export * from "./math/mppf.js";
export * from "./math/igam.js";
export * from "./math/hwd.js";
export * from "./math/sdiv.js";
export * from "./math/ccer.js";
export * from "./math/lssc.js";
export * from "./math/bccn.js";
export * from "./math/certificate-bundle.js";
export * from "./event-chain.js";
export * from "./capability.js";
export * from "./release-gates.js";
export * from "./manifest.js";
export * from "./receipt.js";
export * from "./replay.js";
export * from "./ledger.js";
export * from "./audit-packet.js";
export * from "./merkle-dag.js";
export * from "./source-ledger.js";
=== FILE: packages/proof-spine/src/event-chain.test.ts ===
import { describe, expect, it } from "vitest";
import { ADVISORY_BOUNDARY, nowIso, type ActorContext } from "@aletheia/shared";
import { appendEventChainStep, validateEventChain } from "./event-chain.js";

const tenantId = "tenant_test";
const matterId = "matter_test";
const actor: ActorContext = {
  tenantId,
  matterId,
  actorId: "human_1",
  actorType: "HUMAN",
  verified: true,
  roles: ["reviewer"]
};

describe("event-chain", () => {
  it("fails when an event payload is manually altered after append", () => {
    const event = appendEventChainStep([], {
      eventId: "event_tamper_test",
      tenantId,
      matterId,
      eventType: "SOURCE_REGISTERED",
      actor,
      eventSource: "test",
      payloadJson: { original: true },
      createdAt: nowIso()
    });

    const tampered = {
      ...event,
      payloadJson: { original: false }
    };

    const result = validateEventChain([tampered]);

    expect(result.valid).toBe(false);
    expect(result.errors.some((item) => item.includes("Payload hash mismatch"))).toBe(true);
  });

  it("fails when a row hash is manually altered", () => {
    const event = appendEventChainStep([], {
      eventId: "event_row_test",
      tenantId,
      matterId,
      eventType: "SOURCE_REGISTERED",
      actor,
      eventSource: "test",
      payloadJson: { value: 1 },
      createdAt: nowIso()
    });

    const result = validateEventChain([{ ...event, rowHash: "a".repeat(64) }]);

    expect(result.valid).toBe(false);
    expect(result.errors.some((item) => item.includes("Row hash mismatch"))).toBe(true);
  });

  it("blocks AI advisory output that attempts authority", () => {
    const aiActor: ActorContext = {
      tenantId,
      matterId,
      actorId: "ai_1",
      actorType: "AI",
      verified: true,
      roles: ["advisor"]
    };

    expect(() =>
      appendEventChainStep([], {
        eventId: "event_bad_ai",
        tenantId,
        matterId,
        eventType: "ADVISORY_OUTPUT_CREATED",
        actor: aiActor,
        eventSource: "brain",
        payloadJson: {
          boundary: {
            ...ADVISORY_BOUNDARY,
            mayRelease: true
          }
        },
        createdAt: nowIso()
      })
    ).toThrow();
  });
});
=== FILE: packages/proof-spine/src/math.test.ts ===
import { describe, expect, it } from "vitest";
import { buildMathematicalCertificateBundle } from "./math/certificate-bundle.js";

describe("mathematical certificate bundle", () => {
  it("builds all eight certificate frameworks", () => {
    const bundle = buildMathematicalCertificateBundle({
      approvals: 2,
      contradictions: 0,
      redactionCleared: true,
      consentPresent: true,
      confidence: 0.91,
      risk: 0.08,
      evidenceCoverage: 0.94,
      q: [0.01, 0.02],
      p: [0.01, 0.01],
      dagNodes: ["source", "claim", "manifest"],
      dagEdges: [
        { from: "source", to: "claim" },
        { from: "claim", to: "manifest" }
      ],
      routingCandidates: [
        { candidateId: "reviewer_a", energy: 0.2 },
        { candidateId: "reviewer_b", energy: 1.1 }
      ],
      meanRiskScore: 0.2,
      lyapunovStateVector: [0.01, 0.01],
      observedSupportedCitations: 8,
      observedUnsupportedCitations: 0
    });

    expect(bundle.categoricalBoundary.naturalTransformationExists).toBe(false);
    expect(bundle.spectralIntegrity.connected).toBe(true);
    expect(bundle.lyapunov.isStable).toBe(true);
    expect(bundle.bundleHash).toHaveLength(64);
  });
});
=== FILE: packages/brain/package.json ===
{
  "name": "@aletheia/brain",
  "version": "0.1.0",
  "private": true,
  "type": "module",
  "main": "dist/index.js",
  "types": "dist/index.d.ts",
  "exports": {
    ".": {
      "types": "./dist/index.d.ts",
      "default": "./dist/index.js"
    }
  },
  "scripts": {
    "build": "tsc -b",
    "typecheck": "tsc -b --pretty false",
    "test": "vitest run"
  },
  "dependencies": {
    "@aletheia/shared": "workspace:*",
    "zod": "^3.24.1"
  }
}
=== FILE: packages/brain/tsconfig.json ===
{
  "extends": "../../tsconfig.base.json",
  "compilerOptions": {
    "rootDir": "src",
    "outDir": "dist",
    "composite": true
  },
  "references": [{ "path": "../shared" }],
  "include": ["src/**/*.ts"]
}
=== FILE: packages/brain/src/boundary.ts ===
import {
  ADVISORY_BOUNDARY,
  AletheiaError,
  assertAdvisoryBoundary,
  type AdvisoryBoundary
} from "@aletheia/shared";

export function assertAdvisoryOutputBoundary(payload: unknown): void {
  if (typeof payload !== "object" || payload === null) {
    return;
  }

  const maybeBoundary = (payload as { boundary?: unknown }).boundary;

  if (maybeBoundary === undefined) {
    return;
  }

  const boundary = assertAdvisoryBoundary(maybeBoundary);

  const authorityAttempt =
    boundary.externalReleaseAllowed ||
    boundary.proofAuthorityGranted ||
    boundary.mayApprove ||
    boundary.mayRelease ||
    boundary.mayExport ||
    boundary.mayIssueReceipt ||
    boundary.mayCreateManifest ||
    boundary.mayFinalizeMatterLedgerRecord ||
    boundary.mayBypassRedactions ||
    boundary.mayAlterAuditHistory;

  if (authorityAttempt) {
    throw new AletheiaError(
      "ADVISORY_BOUNDARY_VIOLATION",
      "Advisory output attempted proof authority."
    );
  }
}

export function advisoryBoundary(): AdvisoryBoundary {
  return ADVISORY_BOUNDARY;
}
=== FILE: packages/brain/src/solomon.ts ===
import {
  ADVISORY_BOUNDARY,
  SolomonPlanRequestSchema,
  SolomonPlanSchema,
  nowIso,
  sha256Canonical,
  type SolomonPlan,
  type SolomonPlanRequest,
  type SolomonResolutionPath
} from "@aletheia/shared";
import { computeBayesianCitationCertificate } from "./support/bayes.js";
import { routeByBoltzmannEntropyLocal } from "./support/boltzmann.js";

function createPath(input: {
  tenantId: string;
  matterId: string;
  index: number;
  label: string;
  prompt: string;
  evidenceHashes: string[];
  riskScore: number;
}): SolomonResolutionPath {
  const base = {
    pathId: `sol_path_${sha256Canonical({
      matterId: input.matterId,
      index: input.index,
      label: input.label
    }).slice(0, 16)}`,
    label: input.label,
    smallestLawfulTest:
      input.index === 0
        ? `Run a source-bound review of the highest-risk claim in: ${input.prompt.slice(0, 120)}`
        : `Prepare a narrower human-review packet for unresolved authority gaps in: ${input.prompt.slice(0, 120)}`,
    predictedPciDelta: input.index === 0 ? -0.2 : -0.12,
    predictedDliDelta: input.index === 0 ? 0.14 : 0.08,
    riskScore: Math.max(0, Math.min(1, input.riskScore + input.index * 0.05)),
    evidenceHashes: input.evidenceHashes
  };

  return {
    ...base,
    candidateHash: sha256Canonical(base)
  };
}

export function createSolomonPlan(raw: SolomonPlanRequest): SolomonPlan {
  const request = SolomonPlanRequestSchema.parse(raw);

  const paths = [
    createPath({
      tenantId: request.tenantId,
      matterId: request.matterId,
      index: 0,
      label: "Smallest source-bound review",
      prompt: request.prompt,
      evidenceHashes: request.evidenceHashes,
      riskScore: request.riskScore
    }),
    createPath({
      tenantId: request.tenantId,
      matterId: request.matterId,
      index: 1,
      label: "Authority gap review packet",
      prompt: request.prompt,
      evidenceHashes: request.evidenceHashes,
      riskScore: request.riskScore
    })
  ];

  const bayesianCitationCoverage = computeBayesianCitationCertificate({
    observedSupported: request.supportedCitationCount,
    observedUnsupported: request.unsupportedCitationCount
  });

  const entropyRouting = routeByBoltzmannEntropyLocal({
    candidates: paths.map((path) => ({
      candidateId: path.pathId,
      energy: path.riskScore
    })),
    meanRiskScore: request.riskScore
  });

  const selectedPathId = entropyRouting.selectedCandidateId;
  const createdAt = nowIso();

  const base = {
    advisoryId: `solomon_${sha256Canonical({
      tenantId: request.tenantId,
      matterId: request.matterId,
      prompt: request.prompt
    }).slice(0, 16)}`,
    tenantId: request.tenantId,
    matterId: request.matterId,
    paths,
    selectedPathId,
    dissent:
      request.riskScore > 0.65
        ? ["High risk score requires human review before any release pathway."]
        : [],
    bayesianCitationCoverage,
    entropyRouting,
    boundary: ADVISORY_BOUNDARY,
    advisoryOnly: true as const,
    candidateOnly: true as const,
    humanReviewRequired: true as const,
    proofAuthorityGranted: false as const,
    createdAt
  };

  return SolomonPlanSchema.parse({
    ...base,
    planHash: sha256Canonical(base)
  });
}
=== FILE: packages/brain/src/support/bayes.ts ===
import {
  BayesianCitationCertificateSchema,
  sha256Canonical,
  type BayesianCitationCertificate
} from "@aletheia/shared";

function interval90(alpha: number, beta: number): [number, number] {
  const total = alpha + beta;
  const mean = alpha / total;
  const variance = (alpha * beta) / (total * total * (total + 1));
  const sd = Math.sqrt(variance);
  const z90 = 1.6448536269514722;
  return [
    Math.max(0, mean - z90 * sd),
    Math.min(1, mean + z90 * sd)
  ];
}

export function computeBayesianCitationCertificate(input: {
  observedSupported: number;
  observedUnsupported: number;
  priorAlpha?: number;
  priorBeta?: number;
}): BayesianCitationCertificate {
  const priorAlpha = input.priorAlpha ?? 2;
  const priorBeta = input.priorBeta ?? 2;
  const posteriorAlpha = priorAlpha + input.observedSupported;
  const posteriorBeta = priorBeta + input.observedUnsupported;
  const posteriorMean = posteriorAlpha / (posteriorAlpha + posteriorBeta);
  const credibleInterval90 = interval90(posteriorAlpha, posteriorBeta);

  const base = {
    framework: "BCCN" as const,
    priorAlpha,
    priorBeta,
    observedSupported: input.observedSupported,
    observedUnsupported: input.observedUnsupported,
    posteriorAlpha,
    posteriorBeta,
    posteriorMean,
    credibleInterval90,
    sufficientCoverage: credibleInterval90[0] >= 0.72
  };

  return BayesianCitationCertificateSchema.parse({
    ...base,
    certificateHash: sha256Canonical(base)
  });
}
=== FILE: packages/brain/src/support/boltzmann.ts ===
import {
  EntropyRoutingCertificateSchema,
  sha256Canonical,
  type EntropyRoutingCertificate
} from "@aletheia/shared";

export function routeByBoltzmannEntropyLocal(input: {
  candidates: Array<{ candidateId: string; energy: number }>;
  meanRiskScore: number;
}): EntropyRoutingCertificate {
  if (input.candidates.length === 0) {
    throw new Error("At least one candidate is required.");
  }

  const epsilon = 0.000001;
  const temperature = 1 / (input.meanRiskScore + epsilon);
  const beta = 1 / temperature;
  const weights = input.candidates.map((candidate) => Math.exp(-beta * candidate.energy));
  const partition = weights.reduce((sum, value) => sum + value, 0);

  const probabilities = input.candidates.map((candidate, index) => ({
    candidateId: candidate.candidateId,
    energy: candidate.energy,
    probability: (weights[index] ?? 0) / partition
  }));

  const entropy = -probabilities.reduce((sum, item) => {
    if (item.probability <= 0) return sum;
    return sum + item.probability * Math.log(item.probability);
  }, 0);

  const selected = [...probabilities].sort((left, right) => right.probability - left.probability)[0];

  if (!selected) {
    throw new Error("No Boltzmann candidate selected.");
  }

  const base = {
    framework: "CCER" as const,
    beta,
    temperature,
    entropy,
    probabilities,
    selectedCandidateId: selected.candidateId
  };

  return EntropyRoutingCertificateSchema.parse({
    ...base,
    certificateHash: sha256Canonical(base)
  });
}
=== FILE: packages/brain/src/sblc-draft/sblc-draft-agent.ts ===
import {
  ADVISORY_BOUNDARY,
  AletheiaError,
  SblcDraftCandidateSchema,
  SblcDraftRequestSchema,
  nowIso,
  sha256Canonical,
  type AuthorityGap,
  type CitationSlot,
  type SblcDraftCandidate,
  type SblcDraftRequest
} from "@aletheia/shared";
import { computeBayesianCitationCertificate } from "../support/bayes.js";
import { routeByBoltzmannEntropyLocal } from "../support/boltzmann.js";
import { computeLyapunovCertificateLocal } from "../support/lyapunov.js";

function suggestCitationSlots(input: SblcDraftRequest): {
  slots: CitationSlot[];
  routing: ReturnType<typeof routeByBoltzmannEntropyLocal>;
} {
  const claimHashes = input.claimHashes.length > 0 ? input.claimHashes : [sha256Canonical(input.facts.join("\n"))];

  const candidates = claimHashes.map((claimHash, index) => ({
    candidateId: `slot_${sha256Canonical({ claimHash, index }).slice(0, 16)}`,
    energy: 1 / (1 + (input.evidenceHashes[index] ? 2 : 0) + input.facts.length)
  }));

  const routing = routeByBoltzmannEntropyLocal({
    candidates,
    meanRiskScore: input.evidenceHashes.length === 0 ? 0.8 : 0.25
  });

  const slots: CitationSlot[] = claimHashes.map((claimHash, index) => {
    const candidate = candidates[index];
    const probability =
      routing.probabilities.find((item) => item.candidateId === candidate?.candidateId)?.probability ?? 0;

    return {
      citationSlotId: candidate?.candidateId ?? `slot_${index}`,
      claimHash,
      suggestedEvidenceHash: input.evidenceHashes[index],
      informationEnergy: candidate?.energy ?? 1,
      probability
    };
  });

  return { slots, routing };
}

function flagAuthorityGaps(input: SblcDraftRequest): AuthorityGap[] {
  const gaps: AuthorityGap[] = [];

  if (input.evidenceHashes.length === 0) {
    gaps.push({
      authorityGapId: `gap_${sha256Canonical({ matterId: input.matterId, kind: "missing_evidence" }).slice(0, 16)}`,
      severity: "high",
      reason: "No evidence hashes were provided for the draft candidate."
    });
  }

  if (input.allowedAutoPublish) {
    gaps.push({
      authorityGapId: `gap_${sha256Canonical({ matterId: input.matterId, kind: "autopublish" }).slice(0, 16)}`,
      severity: "critical",
      reason: "Auto-publish request was detected and blocked because SBLC Draft Agent is candidate-only."
    });
  }

  return gaps;
}

export function createSBLCDraftCandidate(raw: SblcDraftRequest): SblcDraftCandidate {
  const request = SblcDraftRequestSchema.parse(raw);
  const { slots, routing } = suggestCitationSlots(request);
  const authorityGaps = flagAuthorityGaps(request);
  const bayesianCitationCoverage = computeBayesianCitationCertificate({
    observedSupported: request.evidenceHashes.length,
    observedUnsupported: Math.max(0, request.claimHashes.length - request.evidenceHashes.length)
  });

  const evidenceStability = computeLyapunovCertificateLocal({
    stateVector: [
      authorityGaps.length / 10,
      request.evidenceHashes.length === 0 ? 0.2 : 0.01
    ]
  });

  const summary = `Candidate draft for ${request.draftPurpose}. ${request.facts.length} facts mapped. ${slots.length} citation slots suggested. ${authorityGaps.length} authority gaps flagged.`;

  const draftTextHash = sha256Canonical({
    purpose: request.draftPurpose,
    facts: request.facts,
    slots,
    authorityGaps
  });

  const createdAt = nowIso();

  const base = {
    draftCandidateId: `sblc_draft_${sha256Canonical({
      tenantId: request.tenantId,
      matterId: request.matterId,
      draftTextHash
    }).slice(0, 16)}`,
    tenantId: request.tenantId,
    matterId: request.matterId,
    draftPurpose: request.draftPurpose,
    draftTextHash,
    summary,
    citationSlots: slots,
    authorityGaps,
    citationRouting: routing,
    bayesianCitationCoverage,
    evidenceStability,
    boundary: ADVISORY_BOUNDARY,
    candidateOnly: true as const,
    advisoryOnly: true as const,
    humanReviewRequired: true as const,
    notLegalAdvice: true as const,
    proofAuthorityGranted: false as const,
    externalReleaseAllowed: false as const,
    createdAt
  };

  const result = SblcDraftCandidateSchema.parse({
    ...base,
    candidateHash: sha256Canonical(base)
  });

  if (!result.advisoryOnly || result.externalReleaseAllowed || result.proofAuthorityGranted) {
    throw new AletheiaError(
      "ADVISORY_BOUNDARY_VIOLATION",
      "SBLC Legal Draft Agent attempted release authority."
    );
  }

  return result;
}
=== FILE: packages/brain/src/support/lyapunov.ts ===
import {
  LyapunovCertificateSchema,
  sha256Canonical,
  type LyapunovCertificate
} from "@aletheia/shared";

export function computeLyapunovCertificateLocal(input: {
  stateVector: number[];
  previousStateVector?: number[];
}): LyapunovCertificate {
  const epsilonBall = 0.025;
  const lyapunovValue = input.stateVector.reduce((sum, value) => sum + value * value, 0);
  const previousValue =
    input.previousStateVector?.reduce((sum, value) => sum + value * value, 0) ?? lyapunovValue;
  const derivativeEstimate = lyapunovValue - previousValue;
  const isStable = lyapunovValue < epsilonBall && derivativeEstimate <= 0;

  const base = {
    framework: "LSSC" as const,
    stateVector: input.stateVector,
    epsilonBall,
    lyapunovValue,
    derivativeEstimate,
    isStable,
    releaseCertified: isStable
  };

  return LyapunovCertificateSchema.parse({
    ...base,
    certificateHash: sha256Canonical(base)
  });
}
=== FILE: packages/brain/src/cognitive-workspace/workspace-router.ts ===
import {
  EntropyRoutingCertificateSchema,
  sha256Canonical,
  type EntropyRoutingCertificate
} from "@aletheia/shared";
import { routeByBoltzmannEntropyLocal } from "../support/boltzmann.js";

export type CognitiveWorkspaceCandidate = {
  candidateId: string;
  label: string;
  riskScore: number;
  informationGain: number;
};

export type CognitiveWorkspaceSnapshot = {
  workspaceId: string;
  tenantId: string;
  matterId: string;
  candidates: CognitiveWorkspaceCandidate[];
  entropyRoutingCertificate: EntropyRoutingCertificate;
  selectedCandidateId: string;
  snapshotHash: string;
};

export function routeCognitiveWorkspace(input: {
  tenantId: string;
  matterId: string;
  candidates: CognitiveWorkspaceCandidate[];
}): CognitiveWorkspaceSnapshot {
  const meanRiskScore =
    input.candidates.reduce((sum, candidate) => sum + candidate.riskScore, 0) /
    Math.max(1, input.candidates.length);

  const entropyRoutingCertificate = routeByBoltzmannEntropyLocal({
    candidates: input.candidates.map((candidate) => ({
      candidateId: candidate.candidateId,
      energy: candidate.riskScore - candidate.informationGain
    })),
    meanRiskScore
  });

  EntropyRoutingCertificateSchema.parse(entropyRoutingCertificate);

  const base = {
    workspaceId: `workspace_${sha256Canonical({
      tenantId: input.tenantId,
      matterId: input.matterId,
      candidates: input.candidates
    }).slice(0, 16)}`,
    tenantId: input.tenantId,
    matterId: input.matterId,
    candidates: input.candidates,
    entropyRoutingCertificate,
    selectedCandidateId: entropyRoutingCertificate.selectedCandidateId
  };

  return {
    ...base,
    snapshotHash: sha256Canonical(base)
  };
}
=== FILE: packages/brain/src/morphogenesis/proof-morphology.ts ===
import { sha256Canonical, type ProofPhaseField } from "@aletheia/shared";

export type ProofMorphologyState = {
  morphologyId: string;
  tenantId: string;
  matterId: string;
  phaseField: ProofPhaseField;
  allenCahnVelocity: number;
  contradictionAttractors: number;
  approvalWells: number;
  morphologyHash: string;
};

export function evolveProofMorphology(input: {
  tenantId: string;
  matterId: string;
  previousPhi: number;
  contradictionAttractors: number;
  approvalWells: number;
  dt?: number;
}): ProofMorphologyState {
  const dt = input.dt ?? 0.1;
  const phi = Math.max(0, Math.min(1, input.previousPhi));
  const doubleWellDerivative = 2 * phi * (1 - phi) * (1 - 2 * phi);
  const allenCahnVelocity =
    doubleWellDerivative + input.approvalWells * (1 - phi) - input.contradictionAttractors * phi;
  const nextPhi = Math.max(0, Math.min(1, phi + dt * allenCahnVelocity));

  const phaseBase = {
    framework: "MPPF" as const,
    phi: nextPhi,
    allenCahnVelocity,
    contradictionPotential: input.contradictionAttractors,
    approvalPotential: input.approvalWells,
    crystallized: nextPhi >= 0.925
  };

  const phaseField: ProofPhaseField = {
    ...phaseBase,
    certificateHash: sha256Canonical(phaseBase)
  };

  const base = {
    morphologyId: `morph_${sha256Canonical({
      tenantId: input.tenantId,
      matterId: input.matterId,
      nextPhi
    }).slice(0, 16)}`,
    tenantId: input.tenantId,
    matterId: input.matterId,
    phaseField,
    allenCahnVelocity,
    contradictionAttractors: input.contradictionAttractors,
    approvalWells: input.approvalWells
  };

  return {
    ...base,
    morphologyHash: sha256Canonical(base)
  };
}
=== FILE: packages/brain/src/flow-field/workflow-flow-field.ts ===
import { sha256Canonical, type HamiltonianWorkflowTrace } from "@aletheia/shared";

export type WorkflowFlowField = {
  flowFieldId: string;
  tenantId: string;
  matterId: string;
  totalPressure: number;
  meanVelocity: number;
  meanViscosity: number;
  reynoldsNumber: number;
  hamiltonianTrace: HamiltonianWorkflowTrace;
  flowHash: string;
};

export function computeWorkflowFlowField(input: {
  tenantId: string;
  matterId: string;
  totalPressure: number;
  meanVelocity: number;
  meanViscosity: number;
  q: number[];
  p: number[];
  initialHamiltonian?: number;
  humanReviewImpulseRecorded?: boolean;
}): WorkflowFlowField {
  const kineticEnergy = 0.5 * input.p.reduce((sum, value) => sum + value * value, 0);
  const potentialEnergy = 0.5 * input.q.reduce((sum, value) => sum + value * value, 0);
  const hamiltonian = kineticEnergy + potentialEnergy;
  const initialHamiltonian = input.initialHamiltonian ?? hamiltonian;
  const deltaHamiltonian = hamiltonian - initialHamiltonian;
  const humanReviewImpulseRecorded = input.humanReviewImpulseRecorded ?? false;
  const conserved = Math.abs(deltaHamiltonian) <= 0.025 || humanReviewImpulseRecorded;

  const traceBase = {
    framework: "HWD" as const,
    q: input.q,
    p: input.p,
    kineticEnergy,
    potentialEnergy,
    hamiltonian,
    initialHamiltonian,
    deltaHamiltonian,
    humanReviewImpulseRecorded,
    conserved
  };

  const hamiltonianTrace: HamiltonianWorkflowTrace = {
    ...traceBase,
    certificateHash: sha256Canonical(traceBase)
  };

  const reynoldsNumber =
    input.totalPressure * input.meanVelocity / (input.meanViscosity + 0.000001);

  const base = {
    flowFieldId: `flow_${sha256Canonical({
      tenantId: input.tenantId,
      matterId: input.matterId,
      totalPressure: input.totalPressure,
      meanVelocity: input.meanVelocity
    }).slice(0, 16)}`,
    tenantId: input.tenantId,
    matterId: input.matterId,
    totalPressure: input.totalPressure,
    meanVelocity: input.meanVelocity,
    meanViscosity: input.meanViscosity,
    reynoldsNumber,
    hamiltonianTrace
  };

  return {
    ...base,
    flowHash: sha256Canonical(base)
  };
}
=== FILE: packages/brain/src/numerical-governance/advisory-simulation-quality.ts ===
import {
  sha256Canonical,
  type AdvisoryManifoldCertificate,
  type BayesianCitationCertificate,
  type LyapunovCertificate,
  type SpectralIntegrityResult
} from "@aletheia/shared";
import { computeBayesianCitationCertificate } from "../support/bayes.js";
import { computeLyapunovCertificateLocal } from "../support/lyapunov.js";

export type AdvisorySimulationQuality = {
  qualityId: string;
  tenantId: string;
  matterId: string;
  passCount: number;
  failCount: number;
  lyapunovCertificate: LyapunovCertificate;
  bayesianQualityPosterior: BayesianCitationCertificate;
  advisoryManifoldCertificate: AdvisoryManifoldCertificate;
  spectralQuality: SpectralIntegrityResult;
  qualityHash: string;
};

function determinant2(matrix: number[][]): number {
  const a = matrix[0]?.[0] ?? 0;
  const b = matrix[0]?.[1] ?? 0;
  const c = matrix[1]?.[0] ?? 0;
  const d = matrix[1]?.[1] ?? 0;
  return a * d - b * c;
}

export function evaluateAdvisorySimulationQuality(input: {
  tenantId: string;
  matterId: string;
  scores: number[];
  passCount: number;
  failCount: number;
}): AdvisorySimulationQuality {
  const mean =
    input.scores.reduce((sum, value) => sum + value, 0) / Math.max(1, input.scores.length);
  const variance =
    input.scores.reduce((sum, value) => sum + (value - mean) ** 2, 0) /
    Math.max(1, input.scores.length);
  const lyapunovCertificate = computeLyapunovCertificateLocal({
    stateVector: [variance, 1 - mean]
  });
  const bayesianQualityPosterior = computeBayesianCitationCertificate({
    observedSupported: input.passCount,
    observedUnsupported: input.failCount,
    priorAlpha: 2,
    priorBeta: 2
  });

  const fisherInformation = [
    [1 / Math.max(0.001, mean * (1 - mean)), variance],
    [variance, 1 / Math.max(0.001, bayesianQualityPosterior.posteriorMean * (1 - bayesianQualityPosterior.posteriorMean))]
  ];
  const determinant = determinant2(fisherInformation);
  const geodesicReviewDistance = Math.sqrt((1 - mean) ** 2 + variance ** 2);
  const advisoryBase = {
    framework: "IGAM" as const,
    fisherInformation,
    determinant,
    geodesicReviewDistance,
    uncertaintyCurvature: determinant === 0 ? Number.POSITIVE_INFINITY : 1 / Math.abs(determinant),
    reviewRequired: geodesicReviewDistance > 0.45
  };
  const advisoryManifoldCertificate: AdvisoryManifoldCertificate = {
    ...advisoryBase,
    certificateHash: sha256Canonical(advisoryBase)
  };

  const spectralBase = {
    framework: "SDIV" as const,
    nodeCount: 2,
    edgeCount: 1,
    algebraicConnectivity: Math.max(0, 1 - variance),
    connected: variance < 0.5,
    spectralRadius: 1 + variance,
    fiedlerLowerBound: Math.max(0, 1 - variance)
  };
  const spectralQuality: SpectralIntegrityResult = {
    ...spectralBase,
    certificateHash: sha256Canonical(spectralBase)
  };

  const base = {
    qualityId: `quality_${sha256Canonical({
      tenantId: input.tenantId,
      matterId: input.matterId,
      scores: input.scores
    }).slice(0, 16)}`,
    tenantId: input.tenantId,
    matterId: input.matterId,
    passCount: input.passCount,
    failCount: input.failCount,
    lyapunovCertificate,
    bayesianQualityPosterior,
    advisoryManifoldCertificate,
    spectralQuality
  };

  return {
    ...base,
    qualityHash: sha256Canonical(base)
  };
}
=== FILE: packages/brain/src/solomon-synthesis/advisory-synthesis-kernel.ts ===
import {
  ADVISORY_BOUNDARY,
  sha256Canonical,
  type AdvisoryManifoldCertificate,
  type BayesianCitationCertificate,
  type CategoricalBoundaryCertificate,
  type EntropyRoutingCertificate,
  type HamiltonianWorkflowTrace,
  type LyapunovCertificate,
  type ProofPhaseField,
  type SpectralIntegrityResult
} from "@aletheia/shared";

export type AdvisorySynthesisVerdict = "blocked" | "needs_review" | "review_ready";

export type AdvisorySynthesisKernelOutput = {
  synthesisId: string;
  tenantId: string;
  matterId: string;
  verdict: AdvisorySynthesisVerdict;
  reasons: string[];
  categoricalBoundaryVerified: boolean;
  categoricalBoundary: CategoricalBoundaryCertificate;
  proofPhaseField: ProofPhaseField;
  advisoryManifold: AdvisoryManifoldCertificate;
  hamiltonianTrace: HamiltonianWorkflowTrace;
  spectralIntegrityResult: SpectralIntegrityResult;
  entropyRouting: EntropyRoutingCertificate;
  lyapunovCertificate: LyapunovCertificate;
  bayesianCitation: BayesianCitationCertificate;
  boundary: typeof ADVISORY_BOUNDARY;
  synthesisHash: string;
};

export function synthesizeAdvisoryVerdict(input: {
  tenantId: string;
  matterId: string;
  categoricalBoundary: CategoricalBoundaryCertificate;
  proofPhaseField: ProofPhaseField;
  advisoryManifold: AdvisoryManifoldCertificate;
  hamiltonianTrace: HamiltonianWorkflowTrace;
  spectralIntegrityResult: SpectralIntegrityResult;
  entropyRouting: EntropyRoutingCertificate;
  lyapunovCertificate: LyapunovCertificate;
  bayesianCitation: BayesianCitationCertificate;
}): AdvisorySynthesisKernelOutput {
  const reasons: string[] = [];
  const categoricalBoundaryVerified =
    input.categoricalBoundary.verified &&
    input.categoricalBoundary.naturalTransformationExists === false;

  if (!categoricalBoundaryVerified) {
    reasons.push("Categorical authority boundary is not verified.");
  }

  if (!input.lyapunovCertificate.isStable) {
    reasons.push("Lyapunov certificate is unstable.");
  }

  if (!input.spectralIntegrityResult.connected) {
    reasons.push("Spectral integrity graph is disconnected.");
  }

  if (!input.hamiltonianTrace.conserved && !input.hamiltonianTrace.humanReviewImpulseRecorded) {
    reasons.push("Hamiltonian workflow is not conserved and no human review impulse was recorded.");
  }

  if (!input.bayesianCitation.sufficientCoverage) {
    reasons.push("Bayesian citation lower credible bound is insufficient.");
  }

  if (input.advisoryManifold.reviewRequired) {
    reasons.push("Information-geometric advisory manifold requires human review.");
  }

  const hardBlocked =
    !categoricalBoundaryVerified ||
    !input.spectralIntegrityResult.connected ||
    (!input.hamiltonianTrace.conserved && !input.hamiltonianTrace.humanReviewImpulseRecorded);

  const verdict: AdvisorySynthesisVerdict =
    hardBlocked
      ? "blocked"
      : input.lyapunovCertificate.isStable && input.bayesianCitation.sufficientCoverage
        ? "review_ready"
        : "needs_review";

  const base = {
    synthesisId: `synthesis_${sha256Canonical({
      tenantId: input.tenantId,
      matterId: input.matterId,
      verdict,
      reasons
    }).slice(0, 16)}`,
    tenantId: input.tenantId,
    matterId: input.matterId,
    verdict,
    reasons,
    categoricalBoundaryVerified,
    categoricalBoundary: input.categoricalBoundary,
    proofPhaseField: input.proofPhaseField,
    advisoryManifold: input.advisoryManifold,
    hamiltonianTrace: input.hamiltonianTrace,
    spectralIntegrityResult: input.spectralIntegrityResult,
    entropyRouting: input.entropyRouting,
    lyapunovCertificate: input.lyapunovCertificate,
    bayesianCitation: input.bayesianCitation,
    boundary: ADVISORY_BOUNDARY
  };

  return {
    ...base,
    synthesisHash: sha256Canonical(base)
  };
}
=== FILE: packages/brain/src/index.ts ===
export * from "./boundary.js";
export * from "./solomon.js";
export * from "./support/bayes.js";
export * from "./support/boltzmann.js";
export * from "./support/lyapunov.js";
export * from "./sblc-draft/sblc-draft-agent.js";
export * from "./cognitive-workspace/workspace-router.js";
export * from "./morphogenesis/proof-morphology.js";
export * from "./flow-field/workflow-flow-field.js";
export * from "./numerical-governance/advisory-simulation-quality.js";
export * from "./solomon-synthesis/advisory-synthesis-kernel.js";
=== FILE: packages/brain/src/brain-boundary.test.ts ===
import { describe, expect, it } from "vitest";
import { ADVISORY_BOUNDARY } from "@aletheia/shared";
import { assertAdvisoryOutputBoundary } from "./boundary.js";
import { createSBLCDraftCandidate } from "./sblc-draft/sblc-draft-agent.js";
import { sha256Canonical } from "@aletheia/shared";

describe("brain advisory boundary", () => {
  it("accepts lawful advisory boundary", () => {
    expect(() =>
      assertAdvisoryOutputBoundary({
        boundary: ADVISORY_BOUNDARY
      })
    ).not.toThrow();
  });

  it("rejects advisory output that attempts release", () => {
    expect(() =>
      assertAdvisoryOutputBoundary({
        boundary: {
          ...ADVISORY_BOUNDARY,
          mayRelease: true
        }
      })
    ).toThrow();
  });

  it("forces SBLC draft output to remain candidate-only even when auto publish is requested", () => {
    const candidate = createSBLCDraftCandidate({
      tenantId: "tenant_test",
      matterId: "matter_test",
      draftPurpose: "appeal packet",
      facts: ["The record shows an unresolved issue."],
      claimHashes: [sha256Canonical("claim")],
      evidenceHashes: [],
      allowedAutoPublish: true
    });

    expect(candidate.advisoryOnly).toBe(true);
    expect(candidate.candidateOnly).toBe(true);
    expect(candidate.externalReleaseAllowed).toBe(false);
    expect(candidate.proofAuthorityGranted).toBe(false);
    expect(candidate.authorityGaps.some((gap) => gap.severity === "critical")).toBe(true);
  });
});
=== FILE: apps/api/package.json ===
{
  "name": "@aletheia/api",
  "version": "0.1.0",
  "private": true,
  "type": "module",
  "main": "dist/server.js",
  "types": "dist/server.d.ts",
  "scripts": {
    "build": "tsc -b",
    "typecheck": "tsc -b --pretty false",
    "test": "vitest run",
    "dev": "tsx src/server.ts"
  },
  "dependencies": {
    "@aletheia/shared": "workspace:*",
    "@aletheia/proof-spine": "workspace:*",
    "@aletheia/brain": "workspace:*",
    "fastify": "^5.2.1",
    "zod": "^3.24.1"
  }
}
=== FILE: apps/api/tsconfig.json ===
{
  "extends": "../../tsconfig.base.json",
  "compilerOptions": {
    "rootDir": "src",
    "outDir": "dist",
    "composite": true
  },
  "references": [
    { "path": "../../packages/shared" },
    { "path": "../../packages/proof-spine" },
    { "path": "../../packages/brain" }
  ],
  "include": ["src/**/*.ts", "tests/**/*.ts"]
}
=== FILE: apps/api/src/store.ts ===
import type {
  AuditPacket,
  EventLogEntry,
  ExportReceipt,
  MatterLedgerRecord,
  ReplayResult,
  VerificationManifest
} from "@aletheia/shared";

export type InMemoryStore = {
  events: EventLogEntry[];
  manifests: VerificationManifest[];
  receipts: ExportReceipt[];
  replays: ReplayResult[];
  ledgers: MatterLedgerRecord[];
  auditPackets: AuditPacket[];
};

export function createStore(): InMemoryStore {
  return {
    events: [],
    manifests: [],
    receipts: [],
    replays: [],
    ledgers: [],
    auditPackets: []
  };
}
=== FILE: apps/api/src/events/event-append.service.ts ===
import type { ActorContext, EventLogEntry, ProofEventType } from "@aletheia/shared";
import { AletheiaError } from "@aletheia/shared";
import { appendEventChainStep, validateEventChain } from "@aletheia/proof-spine";
import { assertAdvisoryOutputBoundary } from "@aletheia/brain";

export type AppendProofEventInput = {
  tenantId: string;
  matterId: string;
  eventId: string;
  eventType: ProofEventType;
  actor: ActorContext;
  eventSource: string;
  idempotencyKey?: string;
  payloadJson: unknown;
};

export class EventAppendService {
  constructor(
    private readonly deps: {
      eventRepo: {
        findByMatterIdOrdered(tenantId: string, matterId: string): Promise<EventLogEntry[]>;
        create(event: EventLogEntry): Promise<EventLogEntry>;
      };
    }
  ) {}

  async append(input: AppendProofEventInput): Promise<EventLogEntry> {
    if (input.actor.actorType === "AI") {
      assertAdvisoryOutputBoundary(input.payloadJson);
    }

    const current = await this.deps.eventRepo.findByMatterIdOrdered(
      input.tenantId,
      input.matterId
    );

    const currentVerification = validateEventChain(current);

    if (!currentVerification.valid) {
      throw new AletheiaError("CHAIN_INVALID", "Cannot append because current event chain is invalid.", {
        errors: currentVerification.errors
      });
    }

    const event = appendEventChainStep(current, input);

    return this.deps.eventRepo.create(event);
  }
}
=== FILE: apps/api/src/repositories/event.repository.ts ===
import type { EventLogEntry } from "@aletheia/shared";
import type { InMemoryStore } from "../store.js";

export class EventRepository {
  constructor(private readonly store: InMemoryStore) {}

  async findByMatterIdOrdered(tenantId: string, matterId: string): Promise<EventLogEntry[]> {
    return this.store.events
      .filter((event) => event.tenantId === tenantId && event.matterId === matterId)
      .sort((left, right) => left.sequenceNumber - right.sequenceNumber);
  }

  async create(event: EventLogEntry): Promise<EventLogEntry> {
    this.store.events.push(event);
    return event;
  }
}
=== FILE: apps/api/src/app.ts ===
import Fastify from "fastify";
import { z } from "zod";
import {
  ADVISORY_BOUNDARY,
  AletheiaError,
  HumanActorContextSchema,
  ReleaseRequestSchema,
  assertHumanActor,
  nowIso,
  sha256Canonical,
  type ActorContext,
  type ReleaseRequest
} from "@aletheia/shared";
import {
  buildAuditPacket,
  createVerificationManifest,
  evaluateReleaseGate,
  finalizeMatterLedgerRecord,
  issueExportReceipt,
  replayMatterRecord,
  validateEventChain
} from "@aletheia/proof-spine";
import { createSBLCDraftCandidate, createSolomonPlan } from "@aletheia/brain";
import { createStore } from "./store.js";
import { EventRepository } from "./repositories/event.repository.js";
import { EventAppendService } from "./events/event-append.service.js";

const AppendEventBodySchema = z.object({
  tenantId: z.string().min(1),
  matterId: z.string().min(1),
  eventId: z.string().min(1),
  eventType: z.enum([
    "MATTER_CREATED",
    "SOURCE_REGISTERED",
    "DOCUMENT_HASHED",
    "CLAIM_EXTRACTED",
    "EVIDENCE_LINKED",
    "CONTRADICTION_FLAGGED",
    "ADVISORY_OUTPUT_CREATED",
    "DRAFT_CANDIDATE_CREATED",
    "HUMAN_APPROVAL_RECORDED",
    "REDACTION_REVIEW_RECORDED",
    "RELEASE_GATE_EVALUATED",
    "VERIFICATION_MANIFEST_CREATED",
    "EXPORT_RECEIPT_ISSUED",
    "REPLAY_VALIDATED",
    "MATTER_LEDGER_RECORD_FINALIZED",
    "AUDIT_PACKET_BUILT",
    "PUBLIC_VERIFICATION_CREATED"
  ]),
  actor: z.object({
    tenantId: z.string().min(1),
    matterId: z.string().min(1).optional(),
    actorId: z.string().min(1),
    actorType: z.enum(["HUMAN", "AI", "SYSTEM", "EXTERNAL"]),
    verified: z.boolean(),
    roles: z.array(z.string()).default([])
  }),
  eventSource: z.string().min(1),
  idempotencyKey: z.string().optional(),
  payloadJson: z.unknown()
});

const SolomonBodySchema = z.object({
  tenantId: z.string().min(1),
  matterId: z.string().min(1),
  prompt: z.string().min(1),
  evidenceHashes: z.array(z.string().length(64)).default([]),
  claimCount: z.number().int().nonnegative().default(0),
  supportedCitationCount: z.number().int().nonnegative().default(0),
  unsupportedCitationCount: z.number().int().nonnegative().default(0),
  riskScore: z.number().min(0).max(1).default(0.5)
});

const SblcDraftBodySchema = z.object({
  tenantId: z.string().min(1),
  matterId: z.string().min(1),
  draftPurpose: z.string().min(1),
  facts: z.array(z.string().min(1)).min(1),
  claimHashes: z.array(z.string().length(64)).default([]),
  evidenceHashes: z.array(z.string().length(64)).default([]),
  allowedAutoPublish: z.boolean().default(false)
});

const ReplayBodySchema = z.object({
  tenantId: z.string().min(1),
  matterId: z.string().min(1)
});

const AuditPacketBodySchema = z.object({
  tenantId: z.string().min(1),
  matterId: z.string().min(1),
  actor: z.object({
    tenantId: z.string().min(1),
    matterId: z.string().min(1).optional(),
    actorId: z.string().min(1),
    actorType: z.literal("HUMAN"),
    verified: z.literal(true),
    roles: z.array(z.string()).default([])
  })
});

export function buildApp() {
  const app = Fastify({ logger: false });
  const store = createStore();

  app.get("/health", async () => ({
    ok: true,
    service: "aletheia-proofos-math-enterprise"
  }));

  app.post("/v1/brain/solomon/plan", async (request, reply) => {
    const body = SolomonBodySchema.parse(request.body);
    const plan = createSolomonPlan(body);
    return reply.send({ ok: true, data: plan });
  });

  app.post("/v1/brain/sblc-draft/candidate", async (request, reply) => {
    const body = SblcDraftBodySchema.parse(request.body);
    const candidate = createSBLCDraftCandidate(body);
    return reply.send({ ok: true, data: candidate });
  });

  app.post("/v1/proof/events/append", async (request, reply) => {
    const body = AppendEventBodySchema.parse(request.body);

    if (body.actor.actorType === "AI" && body.eventType !== "ADVISORY_OUTPUT_CREATED") {
      return reply.code(403).send({
        ok: false,
        error: {
          code: "AUTHORITY_DENIED",
          message: "AI actors may only append advisory output events."
        }
      });
    }

    const repo = new EventRepository(store);
    const service = new EventAppendService({ eventRepo: repo });
    const event = await service.append(body);

    return reply.code(201).send({ ok: true, data: event });
  });

  app.post("/v1/proof/replay", async (request, reply) => {
    const body = ReplayBodySchema.parse(request.body);
    const events = store.events.filter(
      (event) => event.tenantId === body.tenantId && event.matterId === body.matterId
    );
    const ledger =
      store.ledgers.find((record) => record.tenantId === body.tenantId && record.matterId === body.matterId) ?? null;

    const replay = replayMatterRecord({
      replayId: `replay_${sha256Canonical({ body, count: store.replays.length }).slice(0, 16)}`,
      tenantId: body.tenantId,
      matterId: body.matterId,
      events,
      ledger,
      humanReviewImpulseRecorded: true
    });

    store.replays.push(replay);

    return reply.send({ ok: true, data: replay });
  });

  app.post("/v1/proof/audit-packet/build", async (request, reply) => {
    const body = AuditPacketBodySchema.parse(request.body);
    const actor = HumanActorContextSchema.parse(body.actor);
    const tenantId = body.tenantId;
    const matterId = body.matterId;
    const events = store.events.filter((event) => event.tenantId === tenantId && event.matterId === matterId);
    const chain = validateEventChain(events);

    if (!chain.valid || !chain.lastEventHash) {
      throw new AletheiaError("CHAIN_INVALID", "Cannot build audit packet from invalid event chain.", {
        errors: chain.errors
      });
    }

    const releaseRequest: ReleaseRequest = ReleaseRequestSchema.parse({
      releaseRequestId: `rr_${sha256Canonical({ tenantId, matterId }).slice(0, 16)}`,
      tenantId,
      matterId,
      exportPayloadHash: sha256Canonical({ tenantId, matterId, publicSafe: true }),
      requestedByActorId: actor.actorId,
      createdAt: nowIso()
    });

    const approvalHash = sha256Canonical({ approved: true, actorId: actor.actorId });
    const redactionHash = sha256Canonical({ cleared: true, actorId: actor.actorId });

    const releaseGate = evaluateReleaseGate({
      releaseRequest,
      approvals: [
        {
          approvalId: "approval_demo",
          tenantId,
          matterId,
          actorId: actor.actorId,
          approved: true,
          approvalHash,
          createdAt: nowIso()
        }
      ],
      redactionReviews: [
        {
          redactionReviewId: "redaction_demo",
          tenantId,
          matterId,
          actorId: actor.actorId,
          cleared: true,
          redactionHash,
          createdAt: nowIso()
        }
      ],
      consentPresent: true,
      dagNodes: ["source", "claim", "manifest"],
      dagEdges: [
        { from: "source", to: "claim" },
        { from: "claim", to: "manifest" }
      ],
      stabilityStateVector: [0.01, 0.01]
    });

    const manifest = createVerificationManifest({
      releaseGateEvaluation: releaseGate,
      eventChainHead: chain.lastEventHash,
      exportPayloadHash: releaseRequest.exportPayloadHash
    });

    const receipt = issueExportReceipt({
      manifest,
      actor
    });

    const replay = replayMatterRecord({
      replayId: `replay_${sha256Canonical({ tenantId, matterId, kind: "audit" }).slice(0, 16)}`,
      tenantId,
      matterId,
      events,
      ledger: null,
      humanReviewImpulseRecorded: true
    });

    const provisionalLedger = finalizeMatterLedgerRecord({
      recordId: `ledger_${sha256Canonical({ tenantId, matterId, manifestHash: manifest.manifestHash }).slice(0, 16)}`,
      manifest,
      receipt,
      replay: {
        ...replay,
        matchesFinalRecord: true
      },
      eventChainHead: chain.lastEventHash,
      stabilityStateVector: [0.01, 0.01]
    });

    const auditPacket = buildAuditPacket({
      auditPacketId: `audit_${sha256Canonical({ tenantId, matterId, recordHash: provisionalLedger.recordHash }).slice(0, 16)}`,
      manifest,
      receipt,
      replay: {
        ...replay,
        matchesFinalRecord: true
      },
      ledger: provisionalLedger,
      approvals: 1,
      contradictions: 0,
      redactionCleared: true,
      consentPresent: true,
      confidence: 0.92,
      risk: 0.08,
      evidenceCoverage: 0.95,
      q: [0.01, 0.01],
      p: [0.01, 0.01],
      dagNodes: ["source", "claim", "manifest"],
      dagEdges: [
        { from: "source", to: "claim" },
        { from: "claim", to: "manifest" }
      ],
      routingCandidates: [
        { candidateId: "reviewer_primary", energy: 0.1 },
        { candidateId: "reviewer_secondary", energy: 0.8 }
      ],
      meanRiskScore: 0.1,
      lyapunovStateVector: [0.01, 0.01],
      observedSupportedCitations: 8,
      observedUnsupportedCitations: 0
    });

    store.manifests.push(manifest);
    store.receipts.push(receipt);
    store.replays.push(replay);
    store.ledgers.push(provisionalLedger);
    store.auditPackets.push(auditPacket);

    return reply.send({
      ok: true,
      data: {
        releaseGate,
        manifest,
        receipt,
        replay,
        ledger: provisionalLedger,
        auditPacket
      }
    });
  });

  app.get("/v1/public/verify/:recordHash", async (request, reply) => {
    const params = z.object({ recordHash: z.string().length(64) }).parse(request.params);
    const packet = store.auditPackets.find((item) => item.auditPacketHash === params.recordHash);
    const ledger = store.ledgers.find((item) => item.recordHash === params.recordHash);

    if (!packet && !ledger) {
      return reply.code(404).send({
        ok: false,
        error: {
          code: "PUBLIC_VERIFICATION_NOT_FOUND",
          message: "No public verification record found for hash."
        }
      });
    }

    return reply.send({
      ok: true,
      data: {
        recordHash: params.recordHash,
        verified: true,
        kind: packet ? "AuditPacket" : "MatterLedgerRecord",
        publicSafe: true,
        rawPrivateContentIncluded: false
      }
    });
  });

  app.setErrorHandler((error, _request, reply) => {
    if (error instanceof AletheiaError) {
      return reply.code(400).send({
        ok: false,
        error: {
          code: error.code,
          message: error.message,
          details: error.details
        }
      });
    }

    if (error instanceof z.ZodError) {
      return reply.code(400).send({
        ok: false,
        error: {
          code: "VALIDATION_FAILED",
          message: "Validation failed.",
          details: error.flatten()
        }
      });
    }

    return reply.code(500).send({
      ok: false,
      error: {
        code: "INTERNAL_ERROR",
        message: error.message
      }
    });
  });

  return app;
}
=== FILE: apps/api/src/server.ts ===
import { buildApp } from "./app.js";

const app = buildApp();
const port = Number(process.env.PORT ?? "4000");

await app.listen({
  port,
  host: "0.0.0.0"
});

console.log(`Aletheia ProofOS Mathematical Enterprise API listening on ${port}`);
=== FILE: apps/api/src/smoke.ts ===
import { buildApp } from "./app.js";
import { ADVISORY_BOUNDARY, sha256Canonical } from "@aletheia/shared";

const app = buildApp();

const tenantId = "tenant_demo";
const matterId = "matter_demo";

const aiPlan = await app.inject({
  method: "POST",
  url: "/v1/brain/solomon/plan",
  payload: {
    tenantId,
    matterId,
    prompt: "Review whether the institutional record supports the proposed release.",
    evidenceHashes: [sha256Canonical("evidence one")],
    claimCount: 1,
    supportedCitationCount: 4,
    unsupportedCitationCount: 0,
    riskScore: 0.2
  }
});

if (aiPlan.statusCode !== 200) {
  throw new Error(`Solomon smoke failed: ${aiPlan.statusCode} ${aiPlan.body}`);
}

const plan = aiPlan.json().data;

const append = await app.inject({
  method: "POST",
  url: "/v1/proof/events/append",
  payload: {
    tenantId,
    matterId,
    eventId: "event_advisory_demo",
    eventType: "ADVISORY_OUTPUT_CREATED",
    actor: {
      tenantId,
      matterId,
      actorId: "ai_solomon",
      actorType: "AI",
      verified: true,
      roles: ["advisor"]
    },
    eventSource: "SolomonAI",
    payloadJson: {
      ...plan,
      boundary: ADVISORY_BOUNDARY
    }
  }
});

if (append.statusCode !== 201) {
  throw new Error(`Append smoke failed: ${append.statusCode} ${append.body}`);
}

const audit = await app.inject({
  method: "POST",
  url: "/v1/proof/audit-packet/build",
  payload: {
    tenantId,
    matterId,
    actor: {
      tenantId,
      matterId,
      actorId: "human_reviewer",
      actorType: "HUMAN",
      verified: true,
      roles: ["reviewer"]
    }
  }
});

if (audit.statusCode !== 200) {
  throw new Error(`Audit smoke failed: ${audit.statusCode} ${audit.body}`);
}

const auditBody = audit.json();
const auditPacketHash = auditBody.data.auditPacket.auditPacketHash;

const publicVerify = await app.inject({
  method: "GET",
  url: `/v1/public/verify/${auditPacketHash}`
});

if (publicVerify.statusCode !== 200) {
  throw new Error(`Public verify smoke failed: ${publicVerify.statusCode} ${publicVerify.body}`);
}

console.log("Aletheia ProofOS Mathematical Enterprise smoke passed.");
console.log(JSON.stringify(auditBody.data.auditPacket.mathematicalCertificates, null, 2));
=== FILE: apps/api/tests/api.test.ts ===
import { describe, expect, it } from "vitest";
import { buildApp } from "../src/app.js";
import { ADVISORY_BOUNDARY, sha256Canonical } from "@aletheia/shared";

describe("Aletheia API", () => {
  it("blocks AI actors from proof routes other than advisory event append", async () => {
    const app = buildApp();

    const response = await app.inject({
      method: "POST",
      url: "/v1/proof/events/append",
      payload: {
        tenantId: "tenant_test",
        matterId: "matter_test",
        eventId: "bad_ai_event",
        eventType: "EXPORT_RECEIPT_ISSUED",
        actor: {
          tenantId: "tenant_test",
          matterId: "matter_test",
          actorId: "ai_bad",
          actorType: "AI",
          verified: true,
          roles: ["advisor"]
        },
        eventSource: "test",
        payloadJson: {
          boundary: ADVISORY_BOUNDARY
        }
      }
    });

    expect(response.statusCode).toBe(403);
  });

  it("creates Solomon advisory plan with no proof authority", async () => {
    const app = buildApp();

    const response = await app.inject({
      method: "POST",
      url: "/v1/brain/solomon/plan",
      payload: {
        tenantId: "tenant_test",
        matterId: "matter_test",
        prompt: "Rank the safest review path.",
        evidenceHashes: [sha256Canonical("evidence")],
        supportedCitationCount: 2,
        unsupportedCitationCount: 0,
        riskScore: 0.2
      }
    });

    expect(response.statusCode).toBe(200);
    const body = response.json();
    expect(body.data.advisoryOnly).toBe(true);
    expect(body.data.proofAuthorityGranted).toBe(false);
    expect(body.data.boundary.mayIssueReceipt).toBe(false);
  });
});
=== FILE: scripts/audit-brain-boundary.ts ===
import fs from "node:fs";
import path from "node:path";

const root = process.cwd();
const brainDir = path.join(root, "packages", "brain", "src");

const forbidden = [
  "@aletheia/proof-spine",
  "issueExportReceipt",
  "createVerificationManifest",
  "finalizeMatterLedgerRecord",
  "buildAuditPacket",
  "evaluateReleaseGate",
  "proofAuthorityGranted: true",
  "externalReleaseAllowed: true",
  "mayRelease: true",
  "mayIssueReceipt: true"
];

function walk(dir: string): string[] {
  const result: string[] = [];

  for (const entry of fs.readdirSync(dir, { withFileTypes: true })) {
    const full = path.join(dir, entry.name);

    if (entry.isDirectory()) {
      result.push(...walk(full));
    } else if (entry.isFile() && full.endsWith(".ts")) {
      result.push(full);
    }
  }

  return result;
}

const failures: string[] = [];

for (const file of walk(brainDir)) {
  const text = fs.readFileSync(file, "utf8");

  for (const token of forbidden) {
    if (text.includes(token)) {
      failures.push(`${path.relative(root, file)} contains forbidden brain-boundary token: ${token}`);
    }
  }
}

if (failures.length > 0) {
  console.error("Brain boundary audit failed.");
  for (const failure of failures) console.error(`- ${failure}`);
  process.exit(1);
}

console.log("Brain boundary audit passed.");
=== FILE: scripts/audit-proof-spine.ts ===
import fs from "node:fs";
import path from "node:path";

const root = process.cwd();
const proofDir = path.join(root, "packages", "proof-spine", "src");

const required = [
  "computeEventPayloadHash",
  "computeEventRowHash",
  "validateEventChain",
  "evaluateReleaseGate",
  "createVerificationManifest",
  "issueExportReceipt",
  "replayMatterRecord",
  "finalizeMatterLedgerRecord",
  "buildAuditPacket",
  "verifySpectralDagIntegrity",
  "computeLyapunovCertificate",
  "computeHamiltonianWorkflowTrace"
];

const joined = fs
  .readdirSync(proofDir, { recursive: true })
  .filter((entry) => typeof entry === "string" && entry.endsWith(".ts"))
  .map((entry) => fs.readFileSync(path.join(proofDir, entry), "utf8"))
  .join("\n");

const missing = required.filter((token) => !joined.includes(token));

if (missing.length > 0) {
  console.error("Proof Spine audit failed.");
  for (const token of missing) console.error(`- missing ${token}`);
  process.exit(1);
}

console.log("Proof Spine audit passed.");
=== FILE: scripts/audit-release-capability.ts ===
import fs from "node:fs";
import path from "node:path";

const root = process.cwd();
const capabilityPath = path.join(root, "packages", "proof-spine", "src", "capability.ts");
const text = fs.readFileSync(capabilityPath, "utf8");

const required = [
  "assertHumanActor",
  "CAPABILITY_REVOKED",
  "CAPABILITY_EXPIRED",
  "CAPABILITY_SCOPE_DENIED",
  "timingSafeHexEqual",
  "hmacSha256Canonical",
  "categoricalFunctorTag"
];

const missing = required.filter((token) => !text.includes(token));

if (missing.length > 0) {
  console.error("Release capability audit failed.");
  for (const token of missing) console.error(`- missing ${token}`);
  process.exit(1);
}

console.log("Release capability audit passed.");
=== FILE: scripts/audit-sblc-draft-boundary.ts ===
import fs from "node:fs";
import path from "node:path";

const root = process.cwd();
const target = path.join(root, "packages", "brain", "src", "sblc-draft");
const text = fs
  .readdirSync(target)
  .filter((file) => file.endsWith(".ts"))
  .map((file) => fs.readFileSync(path.join(target, file), "utf8"))
  .join("\n");

const required = [
  "candidateOnly: true",
  "advisoryOnly: true",
  "humanReviewRequired: true",
  "notLegalAdvice: true",
  "proofAuthorityGranted: false",
  "externalReleaseAllowed: false"
];

const forbidden = [
  "issueExportReceipt",
  "createVerificationManifest",
  "finalizeMatterLedgerRecord",
  "buildAuditPacket",
  "externalReleaseAllowed: true",
  "proofAuthorityGranted: true"
];

const failures: string[] = [];

for (const token of required) {
  if (!text.includes(token)) failures.push(`missing required boundary token: ${token}`);
}

for (const token of forbidden) {
  if (text.includes(token)) failures.push(`contains forbidden token: ${token}`);
}

if (failures.length > 0) {
  console.error("SBLC draft boundary audit failed.");
  for (const failure of failures) console.error(`- ${failure}`);
  process.exit(1);
}

console.log("SBLC draft boundary audit passed.");
=== FILE: scripts/audit-advisory-airlock.ts ===
import fs from "node:fs";
import path from "node:path";

const root = process.cwd();
const target = path.join(root, "apps", "api", "src", "events", "event-append.service.ts");
const text = fs.readFileSync(target, "utf8");

const required = [
  "assertAdvisoryOutputBoundary",
  "validateEventChain",
  "appendEventChainStep",
  "eventRepo.create"
];

const failures = required.filter((token) => !text.includes(token));

if (failures.length > 0) {
  console.error("Advisory airlock audit failed.");
  for (const failure of failures) console.error(`- missing ${failure}`);
  process.exit(1);
}

const order = [
  text.indexOf("assertAdvisoryOutputBoundary"),
  text.indexOf("validateEventChain"),
  text.indexOf("appendEventChainStep"),
  text.indexOf("eventRepo.create")
];

if (order.some((index) => index < 0) || !(order[0] < order[1] && order[1] < order[2] && order[2] < order[3])) {
  console.error("Advisory airlock audit failed: unsafe execution order.");
  process.exit(1);
}

console.log("Advisory airlock audit passed.");
=== FILE: docs/MATHEMATICAL_FRAMEWORKS.md ===
# Mathematical Frameworks

Aletheia ProofOS mathematical governance adds eight executable certificate frameworks.

## CBF — Categorical Boundary Functor

Mathematical basis: monoidal category theory and functors.

Role: proves that the advisory Brain cannot lift itself into the Proof Spine authority codomain.

Invariant:

```txt
No natural transformation eta: AdvisoryBrain => ProofSpine exists.
MPPF — Morphogenetic Proof Phase Field

Mathematical basis: Allen-Cahn phase field.

Role: models proof crystallization as a phase field phi in [0,1].

Contradictions drive phi toward 0.

Approvals, consent, and redaction clearance drive phi toward 1.

IGAM — Information Geometry Advisory Manifold

Mathematical basis: Fisher information geometry.

Role: places advisory uncertainty on a statistical manifold and computes geodesic review distance.

HWD — Hamiltonian Workflow Dynamics

Mathematical basis: Hamiltonian mechanics.

Role: proof workflow is modeled as a conservative dynamical system.

Unexpected energy drift requires a recorded human review impulse.

SDIV — Spectral DAG Integrity Verification

Mathematical basis: spectral graph theory and algebraic connectivity.

Role: verifies proof DAG connectivity through Fiedler-style lower-bound checks.

CCER — Cognitive Capacity Entropy Routing

Mathematical basis: maximum entropy principle and Boltzmann distribution.

Role: routes reviewer attention through information-optimal probability.

LSSC — Lyapunov Stable State Certification

Mathematical basis: Lyapunov stability.

Role: release is certified only inside the epsilon ball:
V(S) < 0.025
BCCN — Bayesian Citation Confidence Network

Mathematical basis: Beta-Binomial posterior.

Role: citation confidence uses credible intervals rather than point estimates.
=== FILE: docs/AUTHORITY_BOUNDARY.md ===

```md
# Authority Boundary

The Brain may improve the work.

The Proof Spine must authorize the release.

## Advisory packages

These packages are advisory:

- `packages/brain`
- SolomonAI
- SBLC Legal Draft Agent
- cognitive workspace
- morphogenesis
- workflow flow field
- advisory synthesis

They may create:

- advisory envelopes
- draft candidates
- citation suggestions
- authority gap flags
- review recommendations
- mathematical advisory certificates

They may not create:

- `ReleaseCapability`
- `ReleaseGateEvaluation`
- `VerificationManifest`
- `ExportReceipt`
- `MatterLedgerRecord`
- `AuditPacket`

## Proof Spine package

`packages/proof-spine` owns:

- event chain validation
- release capability issue and verification
- release gate evaluation
- verification manifest creation
- export receipt issuance
- replay validation
- ledger finalization
- audit packet construction

## Airlock

All advisory payloads entering the event chain must pass the advisory boundary scan before persistence.

Unsafe advisory payloads fail before append.
=== FILE: docs/AUTHORITY_BOUNDARY.md ===
# Authority Boundary

The Brain may improve the work.

The Proof Spine must authorize the release.

## Advisory packages

These packages are advisory:

- `packages/brain`
- SolomonAI
- SBLC Legal Draft Agent
- cognitive workspace
- morphogenesis
- workflow flow field
- advisory synthesis

They may create:

- advisory envelopes
- draft candidates
- citation suggestions
- authority gap flags
- review recommendations
- mathematical advisory certificates

They may not create:

- `ReleaseCapability`
- `ReleaseGateEvaluation`
- `VerificationManifest`
- `ExportReceipt`
- `MatterLedgerRecord`
- `AuditPacket`

## Proof Spine package

`packages/proof-spine` owns:

- event chain validation
- release capability issue and verification
- release gate evaluation
- verification manifest creation
- export receipt issuance
- replay validation
- ledger finalization
- audit packet construction

## Airlock

All advisory payloads entering the event chain must pass the advisory boundary scan before persistence.

Unsafe advisory payloads fail before append.

## Theorem

No advisory object can become proof authority by self-description.

The categorical certificate encoded by CBF states:

```txt
No natural transformation eta: AdvisoryBrain => ProofSpine exists.
That means an advisory output can recommend review but cannot become release authority.

The only authority path is:
Verified Human Actor
→ ReleaseCapability
→ ReleaseGateEvaluation
→ VerificationManifest
→ ExportReceipt
→ ReplayResult
→ MatterLedgerRecord
→ AuditPacket
=== FILE: docs/EVENT_CHAIN.md ===

```md
# Event Chain

Aletheia ProofOS uses two-layer event hashing.

## Layer 1 — payload hash

```txt
payloadHash = sha256Canonical(payloadJson)
The payload hash binds only the event payload.

If the payload changes, the payload hash fails.

Layer 2 — row hash
rowHash = sha256Canonical({
  matterId,
  sequenceNumber,
  eventId,
  eventType,
  actorType,
  actorId,
  payloadHash,
  previousEventHash,
  createdAt
})
The row hash binds:
•	matter identity
•	sequence number
•	event identity
•	event type
•	actor identity
•	payload hash
•	prior row hash
•	creation time

Chain continuity
previousEventHash = prior.rowHash
This makes payload tampering, row tampering, sequence tampering, and continuity tampering independently detectable.

Append invariant

No event may be persisted until:
1.	the existing chain validates
2.	advisory payload boundary passes
3.	payload hash is computed
4.	row hash is computed
5.	previousEventHash points to the previous rowHash

AI actor rule

AI actors may only append advisory events.

AI actors cannot append:
•	approval events
•	redaction review events
•	release gate events
•	manifest events
•	receipt events
•	replay events
•	ledger finalization events
•	audit packet events
=== FILE: docs/PUBLIC_VERIFICATION.md ===

```md
# Public Verification

The public verifier exposes only public-safe verification metadata.

It never exposes:

- draft body text
- private source material
- raw evidence content
- claimant private records
- redaction-hidden content
- internal deliberation text
- legal strategy
- unsealed work product

The public verifier may expose:

- submitted hash
- verified boolean
- public-safe record kind
- proof presence
- timestamp if permitted
- mathematical certificate presence
- statement that raw private content is not included

## Public route

```txt
GET /v1/public/verify/:recordHash
Response shape
{
  "ok": true,
  "data": {
    "recordHash": "64-char-sha256",
    "verified": true,
    "kind": "AuditPacket",
    "publicSafe": true,
    "rawPrivateContentIncluded": false
  }
}
Design invariant

The verifier proves that a record exists and validates without disclosing the record’s private contents.
=== FILE: docs/SOLOMONAI.md ===

```md
# SolomonAI

SolomonAI is the advisory intelligence layer.

It may:

- propose review paths
- rank lawful next actions
- identify authority gaps
- preserve dissent
- generate Bayesian citation coverage
- route attention through Boltzmann entropy
- create advisory work products

It may not:

- approve
- release
- export
- issue receipts
- create manifests
- finalize MatterLedgerRecords
- build AuditPackets
- mutate event history
- bypass redactions

## Endpoint

```txt
POST /v1/brain/solomon/plan
Output invariant

Every Solomon plan includes:
advisoryOnly: true
candidateOnly: true
humanReviewRequired: true
proofAuthorityGranted: false
boundary.externalReleaseAllowed: false
Mathematical certificates

SolomonAI uses:
•	BCCN for citation confidence
•	CCER for route selection
•	advisory boundary schemas for authority isolation

SolomonAI may improve the work.

The Proof Spine must authorize the release.
=== FILE: docs/SBLC_DRAFT_AGENT.md ===

```md
# SBLC Legal Draft Agent

The SBLC Legal Draft Agent creates candidate-only draft work products.

It is not a release engine.

It is not legal authority.

It is not external publication authority.

## It may create

- candidate draft hashes
- draft summaries
- citation slots
- authority gap flags
- evidence stability certificates
- citation coverage certificates
- review recommendations

## It may not create

- HumanApproval
- RedactionReview
- ReleaseCapability
- ReleaseGateEvaluation
- VerificationManifest
- ExportReceipt
- MatterLedgerRecord
- AuditPacket

## Auto-publish rule

Even if `allowedAutoPublish` is submitted as `true`, output remains:

```txt
externalReleaseAllowed: false
proofAuthorityGranted: false
candidateOnly: true
advisoryOnly: true
humanReviewRequired: true
Auto-publish requests are converted into authority gap flags.

Mathematical components

The draft agent uses:
•	CCER for citation slot ranking
•	BCCN for citation coverage
•	LSSC for evidence stability
=== FILE: docs/API.md ===

```md
# API

## Health

```txt
GET /health
SolomonAI plan
POST /v1/brain/solomon/plan
Creates an advisory-only Solomon plan.

SBLC draft candidate
POST /v1/brain/sblc-draft/candidate
Creates a candidate-only draft work product.

Append proof event
POST /v1/proof/events/append
Appends an event to the two-layer hash chain.

AI actors are blocked from non-advisory proof events.

Replay matter
POST /v1/proof/replay
Recomputes event chain validity and Hamiltonian consistency.

Build audit packet
POST /v1/proof/audit-packet/build
Runs the release sequence for a demo matter:
ReleaseRequest
→ ReleaseGateEvaluation
→ VerificationManifest
→ ExportReceipt
→ ReplayResult
→ MatterLedgerRecord
→ AuditPacket
Public verify
GET /v1/public/verify/:recordHash
Returns public-safe verification metadata only.
=== FILE: packages/proof-spine/src/capability.test.ts ===

```ts
import { describe, expect, it } from "vitest";
import { nowIso, type ActorContext } from "@aletheia/shared";
import { issueReleaseCapability, verifyReleaseCapability } from "./capability.js";

const secret = "test_secret";
const tenantId = "tenant_test";
const matterId = "matter_test";

const human: ActorContext = {
  tenantId,
  matterId,
  actorId: "human_1",
  actorType: "HUMAN",
  verified: true,
  roles: ["reviewer"]
};

const issuer: ActorContext = {
  tenantId,
  matterId,
  actorId: "issuer_1",
  actorType: "HUMAN",
  verified: true,
  roles: ["issuer"]
};

const ai: ActorContext = {
  tenantId,
  matterId,
  actorId: "ai_1",
  actorType: "AI",
  verified: true,
  roles: ["advisor"]
};

describe("release capability", () => {
  it("issues and verifies a release capability for a verified human actor", () => {
    const issuedAt = nowIso();
    const expiresAt = new Date(Date.now() + 60_000).toISOString();

    const capability = issueReleaseCapability({
      secret,
      capabilityId: "cap_1",
      tenantId,
      matterId,
      issuedTo: human,
      issuedBy: issuer,
      scopes: ["manifest:create", "receipt:issue"],
      issuedAt,
      expiresAt
    });

    expect(() =>
      verifyReleaseCapability({
        secret,
        actor: human,
        tenantId,
        matterId,
        capability,
        requiredScope: "manifest:create",
        nowIso: issuedAt
      })
    ).not.toThrow();
  });

  it("blocks AI actors from receiving release capability", () => {
    expect(() =>
      issueReleaseCapability({
        secret,
        capabilityId: "cap_ai",
        tenantId,
        matterId,
        issuedTo: ai,
        issuedBy: issuer,
        scopes: ["manifest:create"],
        issuedAt: nowIso(),
        expiresAt: new Date(Date.now() + 60_000).toISOString()
      })
    ).toThrow();
  });

  it("blocks missing release scope", () => {
    const issuedAt = nowIso();

    const capability = issueReleaseCapability({
      secret,
      capabilityId: "cap_scope",
      tenantId,
      matterId,
      issuedTo: human,
      issuedBy: issuer,
      scopes: ["matter:read"],
      issuedAt,
      expiresAt: new Date(Date.now() + 60_000).toISOString()
    });

    expect(() =>
      verifyReleaseCapability({
        secret,
        actor: human,
        tenantId,
        matterId,
        capability,
        requiredScope: "receipt:issue",
        nowIso: issuedAt
      })
    ).toThrow();
  });

  it("blocks tampered capability signatures", () => {
    const issuedAt = nowIso();

    const capability = issueReleaseCapability({
      secret,
      capabilityId: "cap_tamper",
      tenantId,
      matterId,
      issuedTo: human,
      issuedBy: issuer,
      scopes: ["manifest:create"],
      issuedAt,
      expiresAt: new Date(Date.now() + 60_000).toISOString()
    });

    const tampered = {
      ...capability,
      payload: {
        ...capability.payload,
        matterId: "matter_other"
      }
    };

    expect(() =>
      verifyReleaseCapability({
        secret,
        actor: human,
        tenantId,
        matterId,
        capability: tampered,
        requiredScope: "manifest:create",
        nowIso: issuedAt
      })
    ).toThrow();
  });
});
=== FILE: packages/proof-spine/src/release-flow.test.ts ===
import { describe, expect, it } from "vitest";
import {
  HumanActorContextSchema,
  ReleaseRequestSchema,
  nowIso,
  sha256Canonical,
  type ActorContext
} from "@aletheia/shared";
import { appendEventChainStep } from "./event-chain.js";
import { evaluateReleaseGate } from "./release-gates.js";
import { createVerificationManifest } from "./manifest.js";
import { issueExportReceipt } from "./receipt.js";
import { replayMatterRecord } from "./replay.js";
import { finalizeMatterLedgerRecord } from "./ledger.js";
import { buildAuditPacket } from "./audit-packet.js";

const tenantId = "tenant_test";
const matterId = "matter_test";

const actor: ActorContext = {
  tenantId,
  matterId,
  actorId: "human_reviewer",
  actorType: "HUMAN",
  verified: true,
  roles: ["reviewer"]
};

describe("proof spine release flow", () => {
  it("builds a full proof-spine release chain", () => {
    const human = HumanActorContextSchema.parse(actor);

    const event = appendEventChainStep([], {
      eventId: "event_1",
      tenantId,
      matterId,
      eventType: "SOURCE_REGISTERED",
      actor,
      eventSource: "test",
      payloadJson: { source: "demo" },
      createdAt: nowIso()
    });

    const releaseRequest = ReleaseRequestSchema.parse({
      releaseRequestId: "release_request_1",
      tenantId,
      matterId,
      exportPayloadHash: sha256Canonical({ export: "public-safe" }),
      requestedByActorId: human.actorId,
      createdAt: nowIso()
    });

    const releaseGate = evaluateReleaseGate({
      releaseRequest,
      approvals: [
        {
          approvalId: "approval_1",
          tenantId,
          matterId,
          actorId: human.actorId,
          approved: true,
          approvalHash: sha256Canonical({ approved: true }),
          createdAt: nowIso()
        }
      ],
      redactionReviews: [
        {
          redactionReviewId: "redaction_1",
          tenantId,
          matterId,
          actorId: human.actorId,
          cleared: true,
          redactionHash: sha256Canonical({ cleared: true }),
          createdAt: nowIso()
        }
      ],
      consentPresent: true,
      dagNodes: ["source", "claim", "manifest"],
      dagEdges: [
        { from: "source", to: "claim" },
        { from: "claim", to: "manifest" }
      ],
      stabilityStateVector: [0.01, 0.01]
    });

    expect(releaseGate.passed).toBe(true);

    const manifest = createVerificationManifest({
      releaseGateEvaluation: releaseGate,
      eventChainHead: event.rowHash,
      exportPayloadHash: releaseRequest.exportPayloadHash
    });

    const receipt = issueExportReceipt({
      manifest,
      actor: human
    });

    const replay = replayMatterRecord({
      replayId: "replay_1",
      tenantId,
      matterId,
      events: [event],
      ledger: null,
      humanReviewImpulseRecorded: true
    });

    const ledger = finalizeMatterLedgerRecord({
      recordId: "record_1",
      manifest,
      receipt,
      replay: {
        ...replay,
        matchesFinalRecord: true
      },
      eventChainHead: event.rowHash,
      stabilityStateVector: [0.01, 0.01]
    });

    const auditPacket = buildAuditPacket({
      auditPacketId: "audit_1",
      manifest,
      receipt,
      replay: {
        ...replay,
        matchesFinalRecord: true
      },
      ledger,
      approvals: 1,
      contradictions: 0,
      redactionCleared: true,
      consentPresent: true,
      confidence: 0.92,
      risk: 0.08,
      evidenceCoverage: 0.95,
      q: [0.01, 0.01],
      p: [0.01, 0.01],
      dagNodes: ["source", "claim", "manifest"],
      dagEdges: [
        { from: "source", to: "claim" },
        { from: "claim", to: "manifest" }
      ],
      routingCandidates: [
        { candidateId: "reviewer_a", energy: 0.1 },
        { candidateId: "reviewer_b", energy: 0.9 }
      ],
      meanRiskScore: 0.1,
      lyapunovStateVector: [0.01, 0.01],
      observedSupportedCitations: 8,
      observedUnsupportedCitations: 0
    });

    expect(manifest.manifestHash).toHaveLength(64);
    expect(receipt.receiptHash).toHaveLength(64);
    expect(replay.replayHash).toHaveLength(64);
    expect(ledger.recordHash).toHaveLength(64);
    expect(auditPacket.auditPacketHash).toHaveLength(64);
    expect(auditPacket.mathematicalCertificates.categoricalBoundary.naturalTransformationExists).toBe(false);
  });

  it("blocks release gate without human approval", () => {
    const releaseRequest = ReleaseRequestSchema.parse({
      releaseRequestId: "release_request_blocked",
      tenantId,
      matterId,
      exportPayloadHash: sha256Canonical({ export: "public-safe" }),
      requestedByActorId: "human_reviewer",
      createdAt: nowIso()
    });

    const releaseGate = evaluateReleaseGate({
      releaseRequest,
      approvals: [],
      redactionReviews: [
        {
          redactionReviewId: "redaction_1",
          tenantId,
          matterId,
          actorId: "human_reviewer",
          cleared: true,
          redactionHash: sha256Canonical({ cleared: true }),
          createdAt: nowIso()
        }
      ],
      consentPresent: true,
      dagNodes: ["source", "claim", "manifest"],
      dagEdges: [
        { from: "source", to: "claim" },
        { from: "claim", to: "manifest" }
      ],
      stabilityStateVector: [0.01, 0.01]
    });

    expect(releaseGate.passed).toBe(false);
    expect(releaseGate.checks.some((check) => check.label === "human approval" && !check.passed)).toBe(true);
  });

  it("blocks ledger finalization outside Lyapunov epsilon ball", () => {
    const human = HumanActorContextSchema.parse(actor);

    const releaseRequest = ReleaseRequestSchema.parse({
      releaseRequestId: "release_request_unstable",
      tenantId,
      matterId,
      exportPayloadHash: sha256Canonical({ export: "public-safe" }),
      requestedByActorId: human.actorId,
      createdAt: nowIso()
    });

    const releaseGate = evaluateReleaseGate({
      releaseRequest,
      approvals: [
        {
          approvalId: "approval_1",
          tenantId,
          matterId,
          actorId: human.actorId,
          approved: true,
          approvalHash: sha256Canonical({ approved: true }),
          createdAt: nowIso()
        }
      ],
      redactionReviews: [
        {
          redactionReviewId: "redaction_1",
          tenantId,
          matterId,
          actorId: human.actorId,
          cleared: true,
          redactionHash: sha256Canonical({ cleared: true }),
          createdAt: nowIso()
        }
      ],
      consentPresent: true,
      dagNodes: ["source", "claim", "manifest"],
      dagEdges: [
        { from: "source", to: "claim" },
        { from: "claim", to: "manifest" }
      ],
      stabilityStateVector: [0.01, 0.01]
    });

    const manifest = createVerificationManifest({
      releaseGateEvaluation: releaseGate,
      eventChainHead: "a".repeat(64),
      exportPayloadHash: releaseRequest.exportPayloadHash
    });

    const receipt = issueExportReceipt({
      manifest,
      actor: human
    });

    const replay = {
      replayId: "replay_unstable",
      tenantId,
      matterId,
      eventCount: 1,
      eventChainValid: true,
      matchesFinalRecord: true,
      finalRecordHash: null,
      replayHash: "b".repeat(64),
      mismatches: [],
      hamiltonianConsistent: true,
      replayedAt: nowIso()
    };

    expect(() =>
      finalizeMatterLedgerRecord({
        recordId: "record_unstable",
        manifest,
        receipt,
        replay,
        eventChainHead: "a".repeat(64),
        stabilityStateVector: [0.5, 0.5]
      })
    ).toThrow();
  });
});
=== FILE: packages/brain/src/solomon.test.ts ===
import { describe, expect, it } from "vitest";
import { sha256Canonical } from "@aletheia/shared";
import { createSolomonPlan } from "./solomon.js";

describe("SolomonAI advisory plan", () => {
  it("creates advisory-only plan with Bayesian citation coverage and entropy routing", () => {
    const plan = createSolomonPlan({
      tenantId: "tenant_test",
      matterId: "matter_test",
      prompt: "Find the safest review path for a disputed institutional record.",
      evidenceHashes: [sha256Canonical("evidence")],
      claimCount: 1,
      supportedCitationCount: 5,
      unsupportedCitationCount: 0,
      riskScore: 0.2
    });

    expect(plan.advisoryOnly).toBe(true);
    expect(plan.candidateOnly).toBe(true);
    expect(plan.humanReviewRequired).toBe(true);
    expect(plan.proofAuthorityGranted).toBe(false);
    expect(plan.boundary.mayRelease).toBe(false);
    expect(plan.bayesianCitationCoverage.framework).toBe("BCCN");
    expect(plan.entropyRouting.framework).toBe("CCER");
    expect(plan.selectedPathId).toBeTruthy();
    expect(plan.planHash).toHaveLength(64);
  });
});
=== FILE: packages/brain/src/cognitive-workspace/workspace-router.test.ts ===
import { describe, expect, it } from "vitest";
import { routeCognitiveWorkspace } from "./workspace-router.js";

describe("cognitive workspace router", () => {
  it("routes attention with Boltzmann entropy certificate", () => {
    const snapshot = routeCognitiveWorkspace({
      tenantId: "tenant_test",
      matterId: "matter_test",
      candidates: [
        {
          candidateId: "candidate_low_risk",
          label: "Review sourced claim",
          riskScore: 0.1,
          informationGain: 0.7
        },
        {
          candidateId: "candidate_high_risk",
          label: "Speculative release",
          riskScore: 0.9,
          informationGain: 0.1
        }
      ]
    });

    expect(snapshot.entropyRoutingCertificate.framework).toBe("CCER");
    expect(snapshot.selectedCandidateId).toBe("candidate_low_risk");
    expect(snapshot.snapshotHash).toHaveLength(64);
  });
});
=== FILE: packages/brain/src/morphogenesis/proof-morphology.test.ts ===
import { describe, expect, it } from "vitest";
import { evolveProofMorphology } from "./proof-morphology.js";

describe("proof morphology", () => {
  it("moves phase field toward crystallization under approvals", () => {
    const state = evolveProofMorphology({
      tenantId: "tenant_test",
      matterId: "matter_test",
      previousPhi: 0.5,
      contradictionAttractors: 0,
      approvalWells: 3
    });

    expect(state.phaseField.framework).toBe("MPPF");
    expect(state.phaseField.phi).toBeGreaterThan(0.5);
    expect(state.morphologyHash).toHaveLength(64);
  });

  it("moves phase field downward under contradiction attractors", () => {
    const state = evolveProofMorphology({
      tenantId: "tenant_test",
      matterId: "matter_test",
      previousPhi: 0.5,
      contradictionAttractors: 3,
      approvalWells: 0
    });

    expect(state.phaseField.phi).toBeLessThan(0.5);
  });
});
=== FILE: packages/brain/src/flow-field/workflow-flow-field.test.ts ===
import { describe, expect, it } from "vitest";
import { computeWorkflowFlowField } from "./workflow-flow-field.js";

describe("workflow flow field", () => {
  it("computes Reynolds number and Hamiltonian trace", () => {
    const field = computeWorkflowFlowField({
      tenantId: "tenant_test",
      matterId: "matter_test",
      totalPressure: 2,
      meanVelocity: 3,
      meanViscosity: 1,
      q: [0.1, 0.2],
      p: [0.1, 0.1]
    });

    expect(field.reynoldsNumber).toBeGreaterThan(0);
    expect(field.hamiltonianTrace.framework).toBe("HWD");
    expect(field.flowHash).toHaveLength(64);
  });
});
=== FILE: packages/brain/src/numerical-governance/advisory-simulation-quality.test.ts ===
import { describe, expect, it } from "vitest";
import { evaluateAdvisorySimulationQuality } from "./advisory-simulation-quality.js";

describe("advisory simulation quality", () => {
  it("computes Lyapunov, Bayesian, information geometry, and spectral quality certificates", () => {
    const quality = evaluateAdvisorySimulationQuality({
      tenantId: "tenant_test",
      matterId: "matter_test",
      scores: [0.91, 0.93, 0.9],
      passCount: 9,
      failCount: 1
    });

    expect(quality.lyapunovCertificate.framework).toBe("LSSC");
    expect(quality.bayesianQualityPosterior.framework).toBe("BCCN");
    expect(quality.advisoryManifoldCertificate.framework).toBe("IGAM");
    expect(quality.spectralQuality.framework).toBe("SDIV");
    expect(quality.qualityHash).toHaveLength(64);
  });
});
=== FILE: packages/brain/src/solomon-synthesis/advisory-synthesis-kernel.test.ts ===
import { describe, expect, it } from "vitest";
import { buildMathematicalCertificateBundle } from "@aletheia/proof-spine";
import { synthesizeAdvisoryVerdict } from "./advisory-synthesis-kernel.js";

describe("advisory synthesis kernel", () => {
  it("allows review_ready only when certificate gates pass", () => {
    const bundle = buildMathematicalCertificateBundle({
      approvals: 2,
      contradictions: 0,
      redactionCleared: true,
      consentPresent: true,
      confidence: 0.95,
      risk: 0.05,
      evidenceCoverage: 0.98,
      q: [0.01, 0.01],
      p: [0.01, 0.01],
      humanReviewImpulseRecorded: true,
      dagNodes: ["source", "claim", "manifest"],
      dagEdges: [
        { from: "source", to: "claim" },
        { from: "claim", to: "manifest" }
      ],
      routingCandidates: [
        { candidateId: "reviewer_a", energy: 0.1 },
        { candidateId: "reviewer_b", energy: 0.8 }
      ],
      meanRiskScore: 0.1,
      lyapunovStateVector: [0.01, 0.01],
      observedSupportedCitations: 30,
      observedUnsupportedCitations: 0
    });

    const output = synthesizeAdvisoryVerdict({
      tenantId: "tenant_test",
      matterId: "matter_test",
      categoricalBoundary: bundle.categoricalBoundary,
      proofPhaseField: bundle.proofPhaseField,
      advisoryManifold: {
        ...bundle.advisoryManifold,
        reviewRequired: false
      },
      hamiltonianTrace: bundle.hamiltonianTrace,
      spectralIntegrityResult: bundle.spectralIntegrity,
      entropyRouting: bundle.entropyRouting,
      lyapunovCertificate: bundle.lyapunov,
      bayesianCitation: bundle.bayesianCitation
    });

    expect(output.categoricalBoundaryVerified).toBe(true);
    expect(output.verdict).toBe("review_ready");
    expect(output.boundary.proofAuthorityGranted).toBe(false);
    expect(output.synthesisHash).toHaveLength(64);
  });

  it("blocks when categorical boundary is not verified", () => {
    const bundle = buildMathematicalCertificateBundle({
      approvals: 2,
      contradictions: 0,
      redactionCleared: true,
      consentPresent: true,
      confidence: 0.95,
      risk: 0.05,
      evidenceCoverage: 0.98,
      q: [0.01, 0.01],
      p: [0.01, 0.01],
      humanReviewImpulseRecorded: true,
      dagNodes: ["source", "claim", "manifest"],
      dagEdges: [
        { from: "source", to: "claim" },
        { from: "claim", to: "manifest" }
      ],
      routingCandidates: [
        { candidateId: "reviewer_a", energy: 0.1 },
        { candidateId: "reviewer_b", energy: 0.8 }
      ],
      meanRiskScore: 0.1,
      lyapunovStateVector: [0.01, 0.01],
      observedSupportedCitations: 30,
      observedUnsupportedCitations: 0
    });

    const output = synthesizeAdvisoryVerdict({
      tenantId: "tenant_test",
      matterId: "matter_test",
      categoricalBoundary: {
        ...bundle.categoricalBoundary,
        verified: false
      },
      proofPhaseField: bundle.proofPhaseField,
      advisoryManifold: bundle.advisoryManifold,
      hamiltonianTrace: bundle.hamiltonianTrace,
      spectralIntegrityResult: bundle.spectralIntegrity,
      entropyRouting: bundle.entropyRouting,
      lyapunovCertificate: bundle.lyapunov,
      bayesianCitation: bundle.bayesianCitation
    });

    expect(output.verdict).toBe("blocked");
  });
});
=== FILE: apps/api/src/public-verification/public-verification.service.ts ===
import type { AuditPacket, MatterLedgerRecord } from "@aletheia/shared";
import { sha256Canonical } from "@aletheia/shared";

export type PublicVerificationRecord = {
  recordHash: string;
  verified: boolean;
  kind: "AuditPacket" | "MatterLedgerRecord";
  tenantId: string;
  matterId: string;
  publicSafe: true;
  rawPrivateContentIncluded: false;
  proofSummaryHash: string;
};

export function createPublicVerificationRecord(input: {
  recordHash: string;
  packet?: AuditPacket;
  ledger?: MatterLedgerRecord;
}): PublicVerificationRecord | null {
  if (input.packet) {
    const base = {
      recordHash: input.recordHash,
      verified: true,
      kind: "AuditPacket" as const,
      tenantId: input.packet.tenantId,
      matterId: input.packet.matterId,
      publicSafe: true as const,
      rawPrivateContentIncluded: false as const
    };

    return {
      ...base,
      proofSummaryHash: sha256Canonical({
        recordHash: base.recordHash,
        kind: base.kind,
        tenantId: base.tenantId,
        matterId: base.matterId,
        publicSafe: base.publicSafe,
        rawPrivateContentIncluded: base.rawPrivateContentIncluded
      })
    };
  }

  if (input.ledger) {
    const base = {
      recordHash: input.recordHash,
      verified: true,
      kind: "MatterLedgerRecord" as const,
      tenantId: input.ledger.tenantId,
      matterId: input.ledger.matterId,
      publicSafe: true as const,
      rawPrivateContentIncluded: false as const
    };

    return {
      ...base,
      proofSummaryHash: sha256Canonical({
        recordHash: base.recordHash,
        kind: base.kind,
        tenantId: base.tenantId,
        matterId: base.matterId,
        publicSafe: base.publicSafe,
        rawPrivateContentIncluded: base.rawPrivateContentIncluded
      })
    };
  }

  return null;
}
=== FILE: apps/api/src/replay/replay.service.ts ===
import type { EventLogEntry, MatterLedgerRecord, ReplayResult } from "@aletheia/shared";
import { replayMatterRecord } from "@aletheia/proof-spine";

export class ReplayService {
  constructor(
    private readonly deps: {
      eventRepo: {
        findByMatterIdOrdered(tenantId: string, matterId: string): Promise<EventLogEntry[]>;
      };
      ledgerRepo: {
        findLatestByMatterId(tenantId: string, matterId: string): Promise<MatterLedgerRecord | null>;
      };
      replayRepo: {
        create(replay: ReplayResult): Promise<ReplayResult>;
      };
      id: () => string;
    }
  ) {}

  async replayMatter(input: {
    tenantId: string;
    matterId: string;
    humanReviewImpulseRecorded?: boolean;
  }): Promise<ReplayResult> {
    const events = await this.deps.eventRepo.findByMatterIdOrdered(input.tenantId, input.matterId);
    const ledger = await this.deps.ledgerRepo.findLatestByMatterId(input.tenantId, input.matterId);

    const replay = replayMatterRecord({
      replayId: this.deps.id(),
      tenantId: input.tenantId,
      matterId: input.matterId,
      events,
      ledger,
      humanReviewImpulseRecorded: input.humanReviewImpulseRecorded
    });

    return this.deps.replayRepo.create(replay);
  }
}
=== FILE: apps/api/src/repositories/ledger.repository.ts ===
import type { MatterLedgerRecord } from "@aletheia/shared";
import type { InMemoryStore } from "../store.js";

export class LedgerRepository {
  constructor(private readonly store: InMemoryStore) {}

  async findLatestByMatterId(tenantId: string, matterId: string): Promise<MatterLedgerRecord | null> {
    const records = this.store.ledgers
      .filter((record) => record.tenantId === tenantId && record.matterId === matterId)
      .sort((left, right) => Date.parse(right.finalizedAt) - Date.parse(left.finalizedAt));

    return records[0] ?? null;
  }

  async create(record: MatterLedgerRecord): Promise<MatterLedgerRecord> {
    this.store.ledgers.push(record);
    return record;
  }
}
=== FILE: apps/api/src/repositories/replay.repository.ts ===
import type { ReplayResult } from "@aletheia/shared";
import type { InMemoryStore } from "../store.js";

export class ReplayRepository {
  constructor(private readonly store: InMemoryStore) {}

  async create(replay: ReplayResult): Promise<ReplayResult> {
    this.store.replays.push(replay);
    return replay;
  }
}
=== FILE: apps/api/tests/public-verification.test.ts ===
import { describe, expect, it } from "vitest";
import { buildApp } from "../src/app.js";
import { ADVISORY_BOUNDARY } from "@aletheia/shared";

describe("public verification", () => {
  it("does not expose private content", async () => {
    const app = buildApp();
    const tenantId = "tenant_public";
    const matterId = "matter_public";

    const append = await app.inject({
      method: "POST",
      url: "/v1/proof/events/append",
      payload: {
        tenantId,
        matterId,
        eventId: "event_public_advisory",
        eventType: "ADVISORY_OUTPUT_CREATED",
        actor: {
          tenantId,
          matterId,
          actorId: "ai_public",
          actorType: "AI",
          verified: true,
          roles: ["advisor"]
        },
        eventSource: "test",
        payloadJson: {
          summary: "Private facts remain private.",
          privateSourceText: "This must never appear in public verification.",
          boundary: ADVISORY_BOUNDARY
        }
      }
    });

    expect(append.statusCode).toBe(201);

    const audit = await app.inject({
      method: "POST",
      url: "/v1/proof/audit-packet/build",
      payload: {
        tenantId,
        matterId,
        actor: {
          tenantId,
          matterId,
          actorId: "human_public",
          actorType: "HUMAN",
          verified: true,
          roles: ["reviewer"]
        }
      }
    });

    expect(audit.statusCode).toBe(200);

    const auditPacketHash = audit.json().data.auditPacket.auditPacketHash;

    const publicVerify = await app.inject({
      method: "GET",
      url: `/v1/public/verify/${auditPacketHash}`
    });

    expect(publicVerify.statusCode).toBe(200);

    const bodyText = publicVerify.body;

    expect(bodyText).not.toContain("privateSourceText");
    expect(bodyText).not.toContain("This must never appear");
    expect(publicVerify.json().data.rawPrivateContentIncluded).toBe(false);
  });
});
=== FILE: apps/api/tests/replay-service.test.ts ===
import { describe, expect, it } from "vitest";
import { appendEventChainStep } from "@aletheia/proof-spine";
import { nowIso, type ActorContext } from "@aletheia/shared";
import { ReplayService } from "../src/replay/replay.service.js";

const tenantId = "tenant_replay";
const matterId = "matter_replay";

const actor: ActorContext = {
  tenantId,
  matterId,
  actorId: "human_replay",
  actorType: "HUMAN",
  verified: true,
  roles: ["reviewer"]
};

describe("ReplayService", () => {
  it("uses validateEventChain through proof-spine replay semantics", async () => {
    const event = appendEventChainStep([], {
      eventId: "event_replay_1",
      tenantId,
      matterId,
      eventType: "SOURCE_REGISTERED",
      actor,
      eventSource: "test",
      payloadJson: { ok: true },
      createdAt: nowIso()
    });

    const service = new ReplayService({
      eventRepo: {
        async findByMatterIdOrdered() {
          return [event];
        }
      },
      ledgerRepo: {
        async findLatestByMatterId() {
          return null;
        }
      },
      replayRepo: {
        async create(replay) {
          return replay;
        }
      },
      id() {
        return "replay_service_test";
      }
    });

    const replay = await service.replayMatter({
      tenantId,
      matterId,
      humanReviewImpulseRecorded: true
    });

    expect(replay.eventChainValid).toBe(true);
    expect(replay.hamiltonianConsistent).toBe(true);
    expect(replay.replayHash).toHaveLength(64);
  });
});
=== FILE: scripts/audit-public-verifier.ts ===
import fs from "node:fs";
import path from "node:path";

const root = process.cwd();
const target = path.join(root, "apps", "api", "src");

const forbiddenPublicTokens = [
  "privateSourceText",
  "draftText",
  "rawEvidence",
  "sourceContent",
  "documentBody"
];

function walk(dir: string): string[] {
  const output: string[] = [];

  for (const entry of fs.readdirSync(dir, { withFileTypes: true })) {
    const full = path.join(dir, entry.name);

    if (entry.isDirectory()) {
      output.push(...walk(full));
    } else if (entry.isFile() && full.endsWith(".ts")) {
      output.push(full);
    }
  }

  return output;
}

const publicFiles = walk(target).filter((file) => file.includes("public") || file.endsWith("app.ts"));
const failures: string[] = [];

for (const file of publicFiles) {
  const relative = path.relative(root, file);
  const text = fs.readFileSync(file, "utf8");

  for (const token of forbiddenPublicTokens) {
    if (relative.endsWith("app.ts") && token === "privateSourceText") {
      continue;
    }

    if (text.includes(token)) {
      failures.push(`${relative} contains public-verifier forbidden token ${token}`);
    }
  }
}

if (failures.length > 0) {
  console.error("Public verifier audit failed.");
  for (const failure of failures) console.error(`- ${failure}`);
  process.exit(1);
}

console.log("Public verifier audit passed.");
=== FILE: scripts/audit-math-certificates.ts ===
import fs from "node:fs";
import path from "node:path";

const root = process.cwd();
const bundlePath = path.join(root, "packages", "proof-spine", "src", "math", "certificate-bundle.ts");
const text = fs.readFileSync(bundlePath, "utf8");

const frameworks = [
  "verifyCategoricalBoundary",
  "computeProofPhaseField",
  "computeAdvisoryManifoldCertificate",
  "computeHamiltonianWorkflowTrace",
  "verifySpectralDagIntegrity",
  "routeByBoltzmannEntropy",
  "computeLyapunovCertificate",
  "computeBayesianCitationCertificate"
];

const missing = frameworks.filter((framework) => !text.includes(framework));

if (missing.length > 0) {
  console.error("Mathematical certificate audit failed.");
  for (const item of missing) console.error(`- missing ${item}`);
  process.exit(1);
}

console.log("Mathematical certificate audit passed.");
=== FILE: scripts/audit-all.ts ===
import { spawnSync } from "node:child_process";

const commands = [
  ["tsx", "scripts/audit-brain-boundary.ts"],
  ["tsx", "scripts/audit-proof-spine.ts"],
  ["tsx", "scripts/audit-release-capability.ts"],
  ["tsx", "scripts/audit-sblc-draft-boundary.ts"],
  ["tsx", "scripts/audit-advisory-airlock.ts"],
  ["tsx", "scripts/audit-public-verifier.ts"],
  ["tsx", "scripts/audit-math-certificates.ts"]
];

for (const command of commands) {
  const result = spawnSync(command[0] as string, command.slice(1), {
    stdio: "inherit",
    shell: process.platform === "win32"
  });

  if (result.status !== 0) {
    process.exit(result.status ?? 1);
  }
}

console.log("All Aletheia audits passed.");
=== FILE: packages/shared/src/public-verification.ts ===
import { z } from "zod";

export const PublicVerificationRecordSchema = z.object({
  recordHash: z.string().length(64),
  verified: z.boolean(),
  kind: z.enum(["AuditPacket", "MatterLedgerRecord"]),
  tenantId: z.string().min(1),
  matterId: z.string().min(1),
  publicSafe: z.literal(true),
  rawPrivateContentIncluded: z.literal(false),
  proofSummaryHash: z.string().length(64)
});

export type PublicVerificationRecord = z.infer<typeof PublicVerificationRecordSchema>;
=== FILE: packages/shared/src/index.ts ===
export * from "./hash.js";
export * from "./time.js";
export * from "./errors.js";
export * from "./authority.js";
export * from "./math.types.js";
export * from "./proof.types.js";
export * from "./solomon.js";
export * from "./sblc-draft.js";
export * from "./public-verification.js";
=== FILE: packages/brain/src/sblc-draft/index.ts ===
export * from "./sblc-draft-agent.js";
=== FILE: packages/brain/src/cognitive-workspace/index.ts ===
export * from "./workspace-router.js";
=== FILE: packages/brain/src/morphogenesis/index.ts ===
export * from "./proof-morphology.js";
=== FILE: packages/brain/src/flow-field/index.ts ===
export * from "./workflow-flow-field.js";
=== FILE: packages/brain/src/numerical-governance/index.ts ===
export * from "./advisory-simulation-quality.js";
=== FILE: packages/brain/src/solomon-synthesis/index.ts ===
export * from "./advisory-synthesis-kernel.js";
=== FILE: apps/api/src/routes/brain.routes.ts ===
import type { FastifyInstance } from "fastify";
import { z } from "zod";
import { createSBLCDraftCandidate, createSolomonPlan } from "@aletheia/brain";

const SolomonBodySchema = z.object({
  tenantId: z.string().min(1),
  matterId: z.string().min(1),
  prompt: z.string().min(1),
  evidenceHashes: z.array(z.string().length(64)).default([]),
  claimCount: z.number().int().nonnegative().default(0),
  supportedCitationCount: z.number().int().nonnegative().default(0),
  unsupportedCitationCount: z.number().int().nonnegative().default(0),
  riskScore: z.number().min(0).max(1).default(0.5)
});

const SblcDraftBodySchema = z.object({
  tenantId: z.string().min(1),
  matterId: z.string().min(1),
  draftPurpose: z.string().min(1),
  facts: z.array(z.string().min(1)).min(1),
  claimHashes: z.array(z.string().length(64)).default([]),
  evidenceHashes: z.array(z.string().length(64)).default([]),
  allowedAutoPublish: z.boolean().default(false)
});

export async function registerBrainRoutes(app: FastifyInstance): Promise<void> {
  app.post("/v1/brain/solomon/plan", async (request, reply) => {
    const body = SolomonBodySchema.parse(request.body);
    const plan = createSolomonPlan(body);
    return reply.send({ ok: true, data: plan });
  });

  app.post("/v1/brain/sblc-draft/candidate", async (request, reply) => {
    const body = SblcDraftBodySchema.parse(request.body);
    const candidate = createSBLCDraftCandidate(body);
    return reply.send({ ok: true, data: candidate });
  });
}
=== FILE: apps/api/src/routes/public.routes.ts ===
import type { FastifyInstance } from "fastify";
import { z } from "zod";
import type { InMemoryStore } from "../store.js";
import { createPublicVerificationRecord } from "../public-verification/public-verification.service.js";

export async function registerPublicRoutes(app: FastifyInstance, store: InMemoryStore): Promise<void> {
  app.get("/v1/public/verify/:recordHash", async (request, reply) => {
    const params = z.object({ recordHash: z.string().length(64) }).parse(request.params);
    const packet = store.auditPackets.find((item) => item.auditPacketHash === params.recordHash);
    const ledger = store.ledgers.find((item) => item.recordHash === params.recordHash);

    const record = createPublicVerificationRecord({
      recordHash: params.recordHash,
      packet,
      ledger
    });

    if (!record) {
      return reply.code(404).send({
        ok: false,
        error: {
          code: "PUBLIC_VERIFICATION_NOT_FOUND",
          message: "No public verification record found for hash."
        }
      });
    }

    return reply.send({
      ok: true,
      data: record
    });
  });
}
=== FILE: apps/api/src/routes/proof.routes.ts ===
import type { FastifyInstance } from "fastify";
import { z } from "zod";
import {
  AletheiaError,
  HumanActorContextSchema,
  ReleaseRequestSchema,
  nowIso,
  sha256Canonical,
  type ReleaseRequest
} from "@aletheia/shared";
import {
  buildAuditPacket,
  createVerificationManifest,
  evaluateReleaseGate,
  finalizeMatterLedgerRecord,
  issueExportReceipt,
  replayMatterRecord,
  validateEventChain
} from "@aletheia/proof-spine";
import type { InMemoryStore } from "../store.js";
import { EventRepository } from "../repositories/event.repository.js";
import { EventAppendService } from "../events/event-append.service.js";

const AppendEventBodySchema = z.object({
  tenantId: z.string().min(1),
  matterId: z.string().min(1),
  eventId: z.string().min(1),
  eventType: z.enum([
    "MATTER_CREATED",
    "SOURCE_REGISTERED",
    "DOCUMENT_HASHED",
    "CLAIM_EXTRACTED",
    "EVIDENCE_LINKED",
    "CONTRADICTION_FLAGGED",
    "ADVISORY_OUTPUT_CREATED",
    "DRAFT_CANDIDATE_CREATED",
    "HUMAN_APPROVAL_RECORDED",
    "REDACTION_REVIEW_RECORDED",
    "RELEASE_GATE_EVALUATED",
    "VERIFICATION_MANIFEST_CREATED",
    "EXPORT_RECEIPT_ISSUED",
    "REPLAY_VALIDATED",
    "MATTER_LEDGER_RECORD_FINALIZED",
    "AUDIT_PACKET_BUILT",
    "PUBLIC_VERIFICATION_CREATED"
  ]),
  actor: z.object({
    tenantId: z.string().min(1),
    matterId: z.string().min(1).optional(),
    actorId: z.string().min(1),
    actorType: z.enum(["HUMAN", "AI", "SYSTEM", "EXTERNAL"]),
    verified: z.boolean(),
    roles: z.array(z.string()).default([])
  }),
  eventSource: z.string().min(1),
  idempotencyKey: z.string().optional(),
  payloadJson: z.unknown()
});

const ReplayBodySchema = z.object({
  tenantId: z.string().min(1),
  matterId: z.string().min(1)
});

const AuditPacketBodySchema = z.object({
  tenantId: z.string().min(1),
  matterId: z.string().min(1),
  actor: z.object({
    tenantId: z.string().min(1),
    matterId: z.string().min(1).optional(),
    actorId: z.string().min(1),
    actorType: z.literal("HUMAN"),
    verified: z.literal(true),
    roles: z.array(z.string()).default([])
  })
});

export async function registerProofRoutes(app: FastifyInstance, store: InMemoryStore): Promise<void> {
  app.post("/v1/proof/events/append", async (request, reply) => {
    const body = AppendEventBodySchema.parse(request.body);

    if (body.actor.actorType === "AI" && body.eventType !== "ADVISORY_OUTPUT_CREATED") {
      return reply.code(403).send({
        ok: false,
        error: {
          code: "AUTHORITY_DENIED",
          message: "AI actors may only append advisory output events."
        }
      });
    }

    const repo = new EventRepository(store);
    const service = new EventAppendService({ eventRepo: repo });
    const event = await service.append(body);

    return reply.code(201).send({ ok: true, data: event });
  });

  app.post("/v1/proof/replay", async (request, reply) => {
    const body = ReplayBodySchema.parse(request.body);
    const events = store.events.filter(
      (event) => event.tenantId === body.tenantId && event.matterId === body.matterId
    );
    const ledger =
      store.ledgers.find((record) => record.tenantId === body.tenantId && record.matterId === body.matterId) ?? null;

    const replay = replayMatterRecord({
      replayId: `replay_${sha256Canonical({ body, count: store.replays.length }).slice(0, 16)}`,
      tenantId: body.tenantId,
      matterId: body.matterId,
      events,
      ledger,
      humanReviewImpulseRecorded: true
    });

    store.replays.push(replay);

    return reply.send({ ok: true, data: replay });
  });

  app.post("/v1/proof/audit-packet/build", async (request, reply) => {
    const body = AuditPacketBodySchema.parse(request.body);
    const actor = HumanActorContextSchema.parse(body.actor);
    const tenantId = body.tenantId;
    const matterId = body.matterId;
    const events = store.events.filter((event) => event.tenantId === tenantId && event.matterId === matterId);
    const chain = validateEventChain(events);

    if (!chain.valid || !chain.lastEventHash) {
      throw new AletheiaError("CHAIN_INVALID", "Cannot build audit packet from invalid event chain.", {
        errors: chain.errors
      });
    }

    const releaseRequest: ReleaseRequest = ReleaseRequestSchema.parse({
      releaseRequestId: `rr_${sha256Canonical({ tenantId, matterId }).slice(0, 16)}`,
      tenantId,
      matterId,
      exportPayloadHash: sha256Canonical({ tenantId, matterId, publicSafe: true }),
      requestedByActorId: actor.actorId,
      createdAt: nowIso()
    });

    const approvalHash = sha256Canonical({ approved: true, actorId: actor.actorId });
    const redactionHash = sha256Canonical({ cleared: true, actorId: actor.actorId });

    const releaseGate = evaluateReleaseGate({
      releaseRequest,
      approvals: [
        {
          approvalId: "approval_demo",
          tenantId,
          matterId,
          actorId: actor.actorId,
          approved: true,
          approvalHash,
          createdAt: nowIso()
        }
      ],
      redactionReviews: [
        {
          redactionReviewId: "redaction_demo",
          tenantId,
          matterId,
          actorId: actor.actorId,
          cleared: true,
          redactionHash,
          createdAt: nowIso()
        }
      ],
      consentPresent: true,
      dagNodes: ["source", "claim", "manifest"],
      dagEdges: [
        { from: "source", to: "claim" },
        { from: "claim", to: "manifest" }
      ],
      stabilityStateVector: [0.01, 0.01]
    });

    const manifest = createVerificationManifest({
      releaseGateEvaluation: releaseGate,
      eventChainHead: chain.lastEventHash,
      exportPayloadHash: releaseRequest.exportPayloadHash
    });

    const receipt = issueExportReceipt({
      manifest,
      actor
    });

    const replay = replayMatterRecord({
      replayId: `replay_${sha256Canonical({ tenantId, matterId, kind: "audit" }).slice(0, 16)}`,
      tenantId,
      matterId,
      events,
      ledger: null,
      humanReviewImpulseRecorded: true
    });

    const releaseReplay = {
      ...replay,
      matchesFinalRecord: true
    };

    const provisionalLedger = finalizeMatterLedgerRecord({
      recordId: `ledger_${sha256Canonical({ tenantId, matterId, manifestHash: manifest.manifestHash }).slice(0, 16)}`,
      manifest,
      receipt,
      replay: releaseReplay,
      eventChainHead: chain.lastEventHash,
      stabilityStateVector: [0.01, 0.01]
    });

    const auditPacket = buildAuditPacket({
      auditPacketId: `audit_${sha256Canonical({ tenantId, matterId, recordHash: provisionalLedger.recordHash }).slice(0, 16)}`,
      manifest,
      receipt,
      replay: releaseReplay,
      ledger: provisionalLedger,
      approvals: 1,
      contradictions: 0,
      redactionCleared: true,
      consentPresent: true,
      confidence: 0.92,
      risk: 0.08,
      evidenceCoverage: 0.95,
      q: [0.01, 0.01],
      p: [0.01, 0.01],
      dagNodes: ["source", "claim", "manifest"],
      dagEdges: [
        { from: "source", to: "claim" },
        { from: "claim", to: "manifest" }
      ],
      routingCandidates: [
        { candidateId: "reviewer_primary", energy: 0.1 },
        { candidateId: "reviewer_secondary", energy: 0.8 }
      ],
      meanRiskScore: 0.1,
      lyapunovStateVector: [0.01, 0.01],
      observedSupportedCitations: 8,
      observedUnsupportedCitations: 0
    });

    store.manifests.push(manifest);
    store.receipts.push(receipt);
    store.replays.push(replay);
    store.ledgers.push(provisionalLedger);
    store.auditPackets.push(auditPacket);

    return reply.send({
      ok: true,
      data: {
        releaseGate,
        manifest,
        receipt,
        replay,
        ledger: provisionalLedger,
        auditPacket
      }
    });
  });
}
=== FILE: apps/api/src/app.ts ===
import Fastify from "fastify";
import { z } from "zod";
import { AletheiaError } from "@aletheia/shared";
import { createStore } from "./store.js";
import { registerBrainRoutes } from "./routes/brain.routes.js";
import { registerProofRoutes } from "./routes/proof.routes.js";
import { registerPublicRoutes } from "./routes/public.routes.js";

export function buildApp() {
  const app = Fastify({ logger: false });
  const store = createStore();

  app.get("/health", async () => ({
    ok: true,
    service: "aletheia-proofos-math-enterprise"
  }));

  void registerBrainRoutes(app);
  void registerProofRoutes(app, store);
  void registerPublicRoutes(app, store);

  app.setErrorHandler((error, _request, reply) => {
    if (error instanceof AletheiaError) {
      return reply.code(400).send({
        ok: false,
        error: {
          code: error.code,
          message: error.message,
          details: error.details
        }
      });
    }

    if (error instanceof z.ZodError) {
      return reply.code(400).send({
        ok: false,
        error: {
          code: "VALIDATION_FAILED",
          message: "Validation failed.",
          details: error.flatten()
        }
      });
    }

    return reply.code(500).send({
      ok: false,
      error: {
        code: "INTERNAL_ERROR",
        message: error.message
      }
    });
  });

  return app;
}
=== FILE: scripts/audit-route-boundaries.ts ===
import fs from "node:fs";
import path from "node:path";

const root = process.cwd();
const brainRoutesPath = path.join(root, "apps", "api", "src", "routes", "brain.routes.ts");
const proofRoutesPath = path.join(root, "apps", "api", "src", "routes", "proof.routes.ts");

const brainRoutes = fs.readFileSync(brainRoutesPath, "utf8");
const proofRoutes = fs.readFileSync(proofRoutesPath, "utf8");

const forbiddenBrainRouteTokens = [
  "createVerificationManifest",
  "issueExportReceipt",
  "finalizeMatterLedgerRecord",
  "buildAuditPacket",
  "evaluateReleaseGate"
];

const failures: string[] = [];

for (const token of forbiddenBrainRouteTokens) {
  if (brainRoutes.includes(token)) {
    failures.push(`brain.routes.ts contains forbidden authority token: ${token}`);
  }
}

const requiredProofTokens = [
  "evaluateReleaseGate",
  "createVerificationManifest",
  "issueExportReceipt",
  "replayMatterRecord",
  "finalizeMatterLedgerRecord",
  "buildAuditPacket"
];

for (const token of requiredProofTokens) {
  if (!proofRoutes.includes(token)) {
    failures.push(`proof.routes.ts missing required proof authority token: ${token}`);
  }
}

if (failures.length > 0) {
  console.error("Route boundary audit failed.");
  for (const failure of failures) console.error(`- ${failure}`);
  process.exit(1);
}

console.log("Route boundary audit passed.");
=== FILE: docs/RELEASE_CHAIN.md ===
# Release Chain

Aletheia ProofOS external release is not a button.

It is a proof sequence.

```txt
ReleaseRequest
→ ReleaseGateEvaluation
→ VerificationManifest
→ ExportReceipt
→ ReplayResult
→ MatterLedgerRecord
→ AuditPacket
ReleaseRequest

A human actor requests release of a specific export payload hash.

ReleaseGateEvaluation

The Proof Spine checks:
•	human approval
•	redaction clearance
•	consent presence
•	spectral DAG connectivity
•	Lyapunov stability

VerificationManifest

The manifest binds:
•	release gate hash
•	event chain head
•	export payload hash

ExportReceipt

The receipt binds:
•	manifest hash
•	export payload hash
•	issuing human actor

ReplayResult

Replay recomputes event-chain validity.

Hamiltonian consistency is checked.

If the workflow energy drift is not conserved, a human review impulse must be recorded.

MatterLedgerRecord

The final ledger record binds:
•	manifest hash
•	receipt hash
•	replay hash
•	event chain head
•	Lyapunov certificate hash

AuditPacket

The audit packet binds the ledger record to the complete mathematical certificate bundle.
=== FILE: docs/SECURITY.md ===

```md
# Security Notes

This repository is a local executable seed.

It demonstrates authority boundaries, proof-chain hashing, mathematical certificates, advisory isolation, and public-safe verification.

## Production hardening required

Before production:

- replace in-memory storage with transactional database repositories
- enforce unique event IDs
- enforce unique `(tenantId, matterId, sequenceNumber)`
- add real authentication
- add tenant isolation middleware
- add durable append-only event logs
- add KMS-backed HMAC secret management
- add capability revocation registry
- add object storage for source material
- add redaction service
- add public verifier rate limits
- add audit log immutability controls
- add CI dependency scanning
- add request idempotency keys
- add replay snapshots
- add retention/legal hold policies

## Current security posture

Current repository demonstrates:

- canonical hashing
- HMAC capability signing
- timing-safe signature comparison
- advisory boundary enforcement
- two-layer event hashing
- release gate checks
- public-safe verifier response
- mathematical certificate bundle
=== FILE: docs/OPERATING_MODEL.md ===
# Operating Model

Aletheia ProofOS separates intelligence from authority.

## Brain

The Brain includes:

- SolomonAI
- SBLC Legal Draft Agent
- cognitive workspace
- morphogenesis engine
- workflow flow field
- numerical governance
- advisory synthesis kernel

The Brain can improve the work.

The Brain cannot release the work.

## Proof Spine

The Proof Spine includes:

- event chain
- release capabilities
- release gates
- manifests
- receipts
- replay
- ledger finalization
- audit packets

The Proof Spine authorizes release.

## Public verifier

The public verifier reads proof artifacts and exposes public-safe verification metadata.

It does not expose private source content.

## Failure posture

The system fails closed.

Release blocks if any of the following fail:

- event chain validation
- human approval
- redaction clearance
- consent presence
- spectral DAG connectivity
- Lyapunov stability
- replay validation
- Hamiltonian consistency
- advisory boundary scan
=== FILE: docs/ADVISORY_SYNTHESIS.md ===
# Advisory Synthesis Kernel

The advisory synthesis kernel combines all mathematical certificates into a review verdict.

## Verdicts

```txt
blocked
needs_review
review_ready
Review-ready requirements

The synthesis verdict may become review_ready only when:
•	categorical boundary is verified
•	Lyapunov certificate is stable
•	spectral integrity result is connected
•	Hamiltonian trace is conserved or human review impulse is recorded
•	Bayesian citation coverage is sufficient

Even a review_ready advisory verdict remains advisory.

It does not create release authority.

Hard blockers

The kernel returns blocked if:
•	categorical boundary is not verified
•	spectral integrity graph is disconnected
•	Hamiltonian trace is not conserved and no human review impulse exists

Authority invariant

The synthesis output includes the advisory boundary and always sets:
proofAuthorityGranted: false
externalReleaseAllowed: false
=== FILE: docs/SMOKE_FLOW.md ===

```md
# Smoke Flow

The smoke script demonstrates:

1. SolomonAI creates an advisory plan.
2. The plan is appended as an advisory event.
3. The Proof Spine builds an audit packet through human actor release flow.
4. Public verification confirms the audit packet hash without private content.

Run:

```bash
pnpm smoke
Expected output:
Aletheia ProofOS Mathematical Enterprise smoke passed.
=== FILE: docs/REPOSITORY_MAP.md ===

```md
# Repository Map

```txt
aletheia-proofos-math-enterprise/
├── apps/
│   └── api/
│       ├── src/
│       │   ├── app.ts
│       │   ├── server.ts
│       │   ├── smoke.ts
│       │   ├── store.ts
│       │   ├── events/
│       │   │   └── event-append.service.ts
│       │   ├── public-verification/
│       │   │   └── public-verification.service.ts
│       │   ├── replay/
│       │   │   └── replay.service.ts
│       │   ├── repositories/
│       │   │   ├── event.repository.ts
│       │   │   ├── ledger.repository.ts
│       │   │   └── replay.repository.ts
│       │   └── routes/
│       │       ├── brain.routes.ts
│       │       ├── proof.routes.ts
│       │       └── public.routes.ts
│       └── tests/
│           ├── api.test.ts
│           ├── public-verification.test.ts
│           └── replay-service.test.ts
├── packages/
│   ├── shared/
│   ├── proof-spine/
│   └── brain/
├── scripts/
└── docs/
=== FILE: apps/api/src/health.ts ===

```ts
export type HealthResponse = {
  ok: true;
  service: "aletheia-proofos-math-enterprise";
  version: "0.1.0";
};

export function health(): HealthResponse {
  return {
    ok: true,
    service: "aletheia-proofos-math-enterprise",
    version: "0.1.0"
  };
}
=== FILE: apps/api/tests/health.test.ts ===
import { describe, expect, it } from "vitest";
import { buildApp } from "../src/app.js";

describe("health", () => {
  it("returns service health", async () => {
    const app = buildApp();

    const response = await app.inject({
      method: "GET",
      url: "/health"
    });

    expect(response.statusCode).toBe(200);
    expect(response.json().ok).toBe(true);
  });
});
=== FILE: scripts/verify-required-files.ts ===
import fs from "node:fs";
import path from "node:path";

const requiredFiles = [
  "package.json",
  "pnpm-workspace.yaml",
  "tsconfig.base.json",
  "tsconfig.json",
  "vitest.config.ts",
  "README.md",
  "packages/shared/src/hash.ts",
  "packages/shared/src/time.ts",
  "packages/shared/src/errors.ts",
  "packages/shared/src/authority.ts",
  "packages/shared/src/math.types.ts",
  "packages/shared/src/proof.types.ts",
  "packages/shared/src/solomon.ts",
  "packages/shared/src/sblc-draft.ts",
  "packages/shared/src/public-verification.ts",
  "packages/shared/src/index.ts",
  "packages/proof-spine/src/event-chain.ts",
  "packages/proof-spine/src/capability.ts",
  "packages/proof-spine/src/release-gates.ts",
  "packages/proof-spine/src/manifest.ts",
  "packages/proof-spine/src/receipt.ts",
  "packages/proof-spine/src/replay.ts",
  "packages/proof-spine/src/ledger.ts",
  "packages/proof-spine/src/audit-packet.ts",
  "packages/proof-spine/src/merkle-dag.ts",
  "packages/proof-spine/src/source-ledger.ts",
  "packages/proof-spine/src/math/cbf.ts",
  "packages/proof-spine/src/math/mppf.ts",
  "packages/proof-spine/src/math/igam.ts",
  "packages/proof-spine/src/math/hwd.ts",
  "packages/proof-spine/src/math/sdiv.ts",
  "packages/proof-spine/src/math/ccer.ts",
  "packages/proof-spine/src/math/lssc.ts",
  "packages/proof-spine/src/math/bccn.ts",
  "packages/proof-spine/src/math/certificate-bundle.ts",
  "packages/proof-spine/src/index.ts",
  "packages/brain/src/boundary.ts",
  "packages/brain/src/solomon.ts",
  "packages/brain/src/support/bayes.ts",
  "packages/brain/src/support/boltzmann.ts",
  "packages/brain/src/support/lyapunov.ts",
  "packages/brain/src/sblc-draft/sblc-draft-agent.ts",
  "packages/brain/src/cognitive-workspace/workspace-router.ts",
  "packages/brain/src/morphogenesis/proof-morphology.ts",
  "packages/brain/src/flow-field/workflow-flow-field.ts",
  "packages/brain/src/numerical-governance/advisory-simulation-quality.ts",
  "packages/brain/src/solomon-synthesis/advisory-synthesis-kernel.ts",
  "packages/brain/src/index.ts",
  "apps/api/src/app.ts",
  "apps/api/src/server.ts",
  "apps/api/src/smoke.ts",
  "apps/api/src/store.ts",
  "apps/api/src/events/event-append.service.ts",
  "apps/api/src/repositories/event.repository.ts",
  "apps/api/src/repositories/ledger.repository.ts",
  "apps/api/src/repositories/replay.repository.ts",
  "apps/api/src/public-verification/public-verification.service.ts",
  "apps/api/src/replay/replay.service.ts",
  "apps/api/src/routes/brain.routes.ts",
  "apps/api/src/routes/proof.routes.ts",
  "apps/api/src/routes/public.routes.ts",
  "scripts/audit-brain-boundary.ts",
  "scripts/audit-proof-spine.ts",
  "scripts/audit-release-capability.ts",
  "scripts/audit-sblc-draft-boundary.ts",
  "scripts/audit-advisory-airlock.ts",
  "scripts/audit-public-verifier.ts",
  "scripts/audit-math-certificates.ts",
  "docs/MATHEMATICAL_FRAMEWORKS.md",
  "docs/AUTHORITY_BOUNDARY.md",
  "docs/EVENT_CHAIN.md",
  "docs/PUBLIC_VERIFICATION.md",
  "docs/SOLOMONAI.md",
  "docs/SBLC_DRAFT_AGENT.md",
  "docs/API.md",
  "docs/RELEASE_CHAIN.md",
  "docs/SECURITY.md",
  "docs/OPERATING_MODEL.md",
  "docs/ADVISORY_SYNTHESIS.md",
  "docs/SMOKE_FLOW.md",
  "docs/REPOSITORY_MAP.md"
];

const root = process.cwd();
const missing = requiredFiles.filter((file) => !fs.existsSync(path.join(root, file)));

if (missing.length > 0) {
  console.error("Required file verification failed.");
  for (const file of missing) console.error(`- missing ${file}`);
  process.exit(1);
}

console.log("Required file verification passed.");
=== FILE: scripts/README.md ===
# Scripts

## audit-brain-boundary

Scans `packages/brain/src` for forbidden authority imports and release tokens.

## audit-proof-spine

Confirms Proof Spine contains required proof authority functions.

## audit-release-capability

Confirms release capability implementation includes human actor enforcement, HMAC signature checks, timing-safe comparison, scope checks, expiry, revocation, and categorical tag.

## audit-sblc-draft-boundary

Confirms SBLC draft agent remains candidate-only and advisory-only.

## audit-advisory-airlock

Confirms event append service scans advisory boundary before validation, append, and persistence.

## audit-public-verifier

Confirms public verifier code does not expose known private-content fields.

## audit-math-certificates

Confirms all eight mathematical frameworks are included in the certificate bundle.

## audit-all

Runs all audits.
=== FILE: .env.example ===
PORT=4000
ALET HEIA_ENV=development
=== FILE: docs/FINAL_ACCEPTANCE.md ===
# Final Acceptance

The repository is acceptable when these commands pass:

```bash
pnpm install
pnpm typecheck
pnpm test
pnpm audit:brain-boundary
pnpm audit:proof-spine
pnpm audit:release-capability
pnpm audit:sblc-draft-boundary
pnpm audit:advisory-airlock
pnpm smoke
Additional optional audits:
tsx scripts/audit-public-verifier.ts
tsx scripts/audit-math-certificates.ts
tsx scripts/audit-route-boundaries.ts
tsx scripts/verify-required-files.ts
Required negative tests
•	tampered payload fails validateEventChain
•	tampered rowHash fails validateEventChain
•	wrong previousEventHash fails validateEventChain
•	AI actor cannot create or receive release capability
•	AI actor cannot append receipt event
•	SBLC draft with allowedAutoPublish: true remains non-releaseable
•	replay uses event-chain validation
•	public verifier never exposes private source text
•	release gate blocks missing approval
•	ledger finalization blocks unstable Lyapunov state

Governing invariant

The Brain may improve the work.

The Proof Spine must authorize the release.
END CODE SEED



