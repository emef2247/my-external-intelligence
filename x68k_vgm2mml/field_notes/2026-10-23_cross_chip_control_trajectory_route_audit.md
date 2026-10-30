# 発音単位・制御カーブと閾値の経路監査（2026-10-23）

静的なコード／ドキュメント／外部FieldNotes監査。変換・コンパイル・試聴・テスト・変換コード変更は行っていない。オプション配線の存在は、その曲で実際に省略が発生することや音響的正しさを意味しない。

## ユーザーの17ms指定の意味

約16.7ms（735samples）の制御区間を整理対象に含めるため、x68k側の--fm-state-ms既定を17ms（749samples）とした。全ての17ms以下の状態が不要という主張ではない。--normalization-msは独立した既定8msの全発音省略・時刻補正。現時点のnative MUCOMは1ms、MSXのOPLL選別はopt-inで固定44samples。

## 経路ごとの現状

### x68k_vgm2mml

| 経路 | 変換経路 | --fm-state-ms／選別 | 既定 |
|---|---|---|---|
| OPM → OPM | 対応 | 接続あり・対象限定 | 17ms |
| OPNA → OPM | 対応（対応機能の範囲） | 接続あり・対象限定 | 17ms |
| PSG/SSG → OPM | 対応（音源機能に制約あり） | 接続あり・対象限定 | 17ms |
| PSG/SSG → PCM | PSG PCM対応 | PCM波形には不適用 | 対象外 |
| SCC → OPM | 対応（波形近似） | 接続あり・対象限定 | 17ms |
| DCSG → OPM | 現行canonical MDX経路では未対応 | 対象経路なし | 対象外 |

- **OPM → OPM**：正規化有効時、structured OPMゲート内のpitch/uniform carrier TL。根拠：`py/opm_conversion.py:33,387; py/opm_short_states.py:17`。
- **OPNA → OPM**：OPNAをOPMへ投影後、共通convert_opm。SSGから投影したFMも対象になり得る。根拠：`py/opna_opm.py:381; py/opm_conversion.py:387`。
- **PSG/SSG → OPM**：AY系PSG/SCC musical routeでOPM投影後に適用。OPNA SSGはopna_opm経由。根拠：`py/psg_scc_conversion.py:239-270; py/opna_opm.py:381`。
- **PSG/SSG → PCM**：AY連続runtimeから直接PCMを生成。同時にSCC→FMがあればFM部分のみ適用。OPNA SSG→PCM一般対応は今回確認していない。根拠：`py/psg_scc_conversion.py:246-255; py/psg_playback.py:138`。
- **SCC → OPM**：SCCのsource note/volume runsを解釈しOPMへ投影後、共通FMフィルタ。根拠：`py/psg_scc_opm.py:212-236; py/psg_scc_conversion.py:239-270`。
- **DCSG → OPM**：SN76489の0x50/0x30入力はAYの0xa0経路とは別。mucom側DCSG実装から自動継承されない。根拠：`py/conversion_config.py:41-43,109-113,148-151`。

### mucom88_vgm2mml

| 経路 | 変換経路 | --fm-state-ms／選別 | 既定 |
|---|---|---|---|
| OPNA → OPNA (native MUCOM) | 対応 | 接続あり・FM部分限定 | 1ms |
| OPM → OPNA (native MUCOM) | 対応（近似・機能制約あり） | 接続あり・対象限定 | 1ms |
| OPLL → OPLL (MUCOMSX) | 対応経路あり・要注意 | 未接続 | 対象外 |
| PSG/SSG → MSX内蔵PSG (MUCOMSX) | AY系PSG対応。OPNA SSG一般入力は別確認が必要 | 未接続 | 対象外 |
| PSG/SSG → OPNA SSG | native MUCOMで対応。MUCOMSXはPSG shadow/DCSG割当など別経路 | 未接続 | 対象外 |
| SCC → OPNA FM (MUCOMSX) | 対応（additive4近似） | 未接続 | 対象外 |
| SCC → SSGExtend/MSXSCC | 現行コンバータから選択する経路なし | 対象経路なし | 対象外 |
| SCC → SCC | 現行コンバータに直接出力なし | 対象経路なし | 対象外 |

- **OPNA → OPNA (native MUCOM)**：normalize_lengths有効時の候補時計算と最終FM gate。SSG/rhythm/PCMには適用しない。根拠：`py/mucom_normalization.py:122-127; py/opna_muc.py:682-701`。
- **OPM → OPNA (native MUCOM)**：OPM→OPNA投影後にnative共通FM gate/filterへ接続。根拠：`py/opna_muc.py:576-610,682-701`。
- **OPLL → OPLL (MUCOMSX)**：OPNA DAT代理音色を経由する現行実装。プリセット番号・custom patchの直接保持保証は別課題。根拠：`py/mucomsx_conversion.py:374-405; py/mucomsx_opll.py:1-5,85-91`。
- **PSG/SSG → MSX内蔵PSG (MUCOMSX)**：MSXD/E/F。既存volume-run/loop/macro処理は存在する。根拠：`py/mucomsx_conversion.py:25-26,188-213,411-462`。
- **PSG/SSG → OPNA SSG**：SSG投影はFM gateフィルタを通らない。MUCOMSX enhancementも独立。根拠：`py/opna_muc.py:627-657; py/mucomsx_conversion.py:207-230`。
- **SCC → OPNA FM (MUCOMSX)**：独立SCC waveform→OPNA FM rendererでnative opna_mucフィルタは通らない。根拠：`py/mucomsx_conversion.py:186-206,327-345`。
- **SCC → SSGExtend/MSXSCC**：MUCOMSX側の機能の存在と、converterの出力実装は別。現行SCCは全てOPNA FM割当。根拠：`py/mucomsx_conversion.py:25-26,186-206,493-497`。
- **SCC → SCC**：SCC source分析は保持されるがnative_scc_target_channels=0。根拠：`py/mucomsx_conversion.py:493-497`。

### msx_vgm2mml

| 経路 | 変換経路 | --fm-state-ms／選別 | 既定 |
|---|---|---|---|
| OPLL → OPLL (MGSDRV) | 対応 | 限定的な既存選別あり・CLI未対応 | 固定44samples≈1ms |
| SCC → SCC (MGSDRV) | 対応 | --fm-state-ms未接続 | 対象外 |
| PSG → PSG (MGSDRV) | 対応 | --fm-state-ms未接続 | 対象外 |

- **OPLL → OPLL (MGSDRV)**：--normalize-lengths有効時だけ共通selectorを使用。通常routeはstate trajectoryをtieへ出力。根拠：`vgm2mml.py:157-158,295-308; py/note_normalization.py:263-327`。
- **SCC → SCC (MGSDRV)**：既存complete-note/SW envelope/source loop/macro処理は適用される。根拠：`py/mml_envelopes.py:142-184,246-288; py/pre_envelope_loops.py:9-50`。
- **PSG → PSG (MGSDRV)**：既存hardware/software envelope/source loop/macro処理は適用される。根拠：`py/mml_envelopes.py:142-184,246-288; py/pre_envelope_loops.py:9-50`。

## 接続ありの意味と制約

x68kはstructured/normalization有効時の出力OPMゲート内部での選別。PSG/SCCのnative source interpretation全体へ17msを適用しているわけではない。音色・パン・非一様TL・global/key/zero-time副作用は保護され、全gate保護で実際の省略が起こらない場合もある。registers/legacy等の非対象notationでは同じ処理にならない。

MUCOMはnative --target mucomのFMゲートが対象。--target mucomsxでは非既定の--fm-state-msはエラーになり、既定1msを黙示的に受けてもフィルタを使っているわけではない。MUCOMSXにも全音符を対象とする--normalization-ms処理はあるが、内部制御の選別とは異なる。

## 共通化の中心となる設計

1. Source State/Events/Segmentsは保持する。FMの実KeyOn/OffとPSG/SCC/DCSGの推定attackを区別し、発音単位へ根拠付きでまとめる。
2. 一つの発音に内部pitch/volume/設定/waveformのtrajectoryを持たせる。変化ごとにscore attackへ昇格させない。役割はscore、演奏表情、音量包絡、音色変更、未分類などを区別し、判断できないものは未分類として保存する。
3. 反復構造は完全な発音単位から先に作る。SW envelopeや制御カーブの共有は格納代表を基準に行い、過去のMSXのloops-before-envelope・exact prefix sharingを維持する。
4. ターゲットが表現できるenvelope/LFO/automation等を利用する。できなければ明示的な制御列へ落とす。タイは持続を保存する出力構文なので、一律除去はしない。
5. 閾値による省略は意味解釈とは独立した、明示的な出力近似。SCCだけでなくPSG/SSG/DCSGでもvolume列そのものがSW envelopeや演奏表情になり得るため、17msを全経路へそのままコピーするだけでは目的を満たさない。

FMは内部の発振波形やEG phaseをサンプリングしてMML化しているわけではない。VGMに明示されたKC/KF/TL等を持続音内部のタイ付きスライスへ記述するfallbackがある。モジュレータTLは音色変化、carrier TLは音量に関わり得るため、数値の変化だけでscore/instrumentを一意には分類できない。

時間単位も共通化の境界になる。原VGMの44,100Hz sample、MSXの60Hz envelope/target tick、正規化後のscore stepは異なる。既存のvolume-run列を再利用する場合もtimebaseを明示し、ms閾値をtarget tick列へそのまま当てない。

## MSXから継承すべき既存原則

既存MSXのPSG/SCCはcomplete note＋volume runs→source loop→stored representative上のenvelope bank選択→stateful rendering→exact loop projection→macroの順序。envelope ID0を確保し共有bankを公平に割当。観測されたprefixがtickごと完全一致する場合だけ共有し、未知のtailやreleaseを創作しない。容量不足時は平坦化せず明示的なv＋tieへfallback。

根拠：msx docs/software_envelopes.md:11-77、docs/melody_patterns.md:10-16、外部notes/2026-10-03_psg_scc_pre_envelope_structure.md。現行extract_notesはpitch writeやvolume rise等を保守的境界とするため、任意のvibrato/portamentoまで意味的に分類済みというわけではない。

## 既存共通モジュールの同期状況

改行を正規化して比較すると、fm_state_runs.py、source_loop_plan.py、loop_structure.py、mml_envelopes.py、pre_envelope_loops.py、opll_note_units.pyの6本は3repoで同一。詳細はshared_module_hashes.csv。既存の中核を全面再移植する状況ではなく、呼び出し経路・タイムベース・target adapter・CLI設定の差を先に整理すべき。ファイル一致だけで各経路が同じ処理を使う証拠にはならない。

ordered_timing.pyとmusical_structure.pyはx68k/MUCOM間では同一、MSX側には同名ファイルがない。MSX側の既存source-loop/performed-pattern経路との責務を確認してから取り込む。名前がないことだけで機能不足とは断定しない。

共通化するのは表現・処理の契約であり、FMの実KeyOnとPSG/SCC/DCSGの推定attackを同一規則に置換しない。論理的にnoteとcurveを分離しても、反復再利用の検証では制御カーブと入口／出口状態を保持する。

## MSXへの逆取り込み候補

実在するバッチ入口はscripts/batch_vgm_to_mgs.py。現在--forceと--fm-state-msはなく、normalize-lengthsのみ転送する。既にconvert.log/compile.log、results.csv/json、失敗時のMML保持・不正MGS削除がある。未整備と断定して作り直す必要はない。

機械的な同期候補：閾値の明示パラメータとバッチ転送、: success/: compilation_failed形式の表示、段階別detailと診断パス、既存raw statusの互換保持。現在のcompile_failed/buffer_error/compiler_setup_error等を失わず、共通statusとreason/legacy_statusへ分離する。MGS特有のkeyon_status等も残す。

--forceはflagだけの移植では不十分。未対応効果・target容量等に対して、何を省略し、時間や後続状態をどう保持し、どの診断を出すかを定義する。入力破損や未解決のコンパイラエラーを単にsuccessへ変えない。x68k/MUCOMのsource/target固有ルールをそのままMGSDRVに持ち込まない。

## 実装順序の提案（未実装）

1. この監査の経路表とscopeを確認。SCC→SSGExtend/SCCやDCSG→OPMの新規routeと、既存routeへの短状態選別は別案件。
2. 既存note/curve/loop処理を基準に、全音源共通のcontrol trajectory解釈の責務を定義。各source adapterの意味を維持し、OPM/OPNA・PSG/SSG・SCC・DCSGの順に接続を点検する。
3. MSXのbatch表示・status/detail/log互換を独立して揃える。
4. MSXのOPLL可変閾値を追加し、その後PSG/SCCの意味的curve処理へ展開する。既存MSXを固定リファレンスとして残す比較手段を先に確保する。

## 重要な別課題

MUCOMSXの現行OPLL出力はOPNA DAT代理音色を経由する。OPLLプリセット番号／custom patchをそのまま保持することは、今回の閾値監査だけでは保証できない。直接保持を望む仕様との整合性は別途点検する必要がある。

この監査では追加実装していない。次は監査結果をユーザーが確認した後にscopeを確定する。

独立レビューで適用範囲・設計上の区別を確認した。実装前の監査として提示する。

外部notesの別repoへ保存。変換コード変更、実行検証、stage/commit/pushなし。他repoの既存変更は保持。
