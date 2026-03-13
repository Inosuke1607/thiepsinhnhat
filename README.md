import React, { useEffect, useMemo, useState } from "react";
import { AnimatePresence, motion } from "framer-motion";

const COLORS = [
  "#FFB3C7",
  "#FFD6A5",
  "#FDFFB6",
  "#CAFFBF",
  "#A0E7E5",
  "#BDB2FF",
  "#FFC6FF",
  "#9BF6FF",
];

function rand(min, max) {
  return Math.random() * (max - min) + min;
}

function Firework({ burst }) {
  return (
    <div className="absolute" style={{ left: `${burst.x}%`, top: `${burst.y}%` }}>
      {burst.particles.map((p, i) => (
        <motion.span
          key={i}
          initial={{ x: 0, y: 0, opacity: 1, scale: 0.5 }}
          animate={{ x: p.dx, y: p.dy, opacity: 0, scale: 1.2 }}
          transition={{ duration: 1.15, ease: "easeOut" }}
          className="absolute block rounded-full shadow-md"
          style={{ width: 10, height: 10, background: p.color }}
        />
      ))}
    </div>
  );
}

function Candle({ left }) {
  return (
    <div className="absolute" style={{ left, top: -54 }}>
      <div className="relative mx-auto h-10 w-3 rounded-full bg-pink-300 shadow-sm">
        <motion.div
          animate={{ scale: [1, 1.15, 0.92, 1], rotate: [-4, 3, -2, 0] }}
          transition={{ duration: 1.2, repeat: Infinity }}
          className="absolute left-1/2 top-0 h-5 w-5 -translate-x-1/2 -translate-y-4"
          style={{
            background:
              "radial-gradient(circle, #FFF7A8 0%, #FFD166 42%, #FF9F1C 78%, rgba(255,159,28,0.15) 100%)",
            borderRadius: "60% 60% 60% 60% / 75% 75% 45% 45%",
            boxShadow: "0 0 18px rgba(255, 176, 59, 0.85)",
          }}
        />
      </div>
    </div>
  );
}

function Tier({ width, height, colors, children }) {
  return (
    <div className="relative mx-auto" style={{ width, height }}>
      <div
        className="absolute inset-0 rounded-[999px] border-4 border-white/60 shadow-xl"
        style={{ background: `linear-gradient(180deg, ${colors[0]}, ${colors[1]})` }}
      />
      <div
        className="absolute left-1/2 top-2 h-4 -translate-x-1/2 rounded-full bg-white/30"
        style={{ width: width * 0.78 }}
      />
      <div className="absolute inset-x-5 bottom-3 flex justify-between">
        {Array.from({ length: 8 }).map((_, i) => (
          <motion.div
            key={i}
            animate={{ y: [0, 6, 0] }}
            transition={{ duration: 1.4 + i * 0.08, repeat: Infinity }}
            className="h-6 w-6 rounded-b-full bg-white/85"
          />
        ))}
      </div>
      {children}
    </div>
  );
}

function Envelope({ open, onOpen }) {
  return (
    <div
      className="relative h-[320px] w-[420px] cursor-pointer select-none [perspective:1400px]"
      onClick={onOpen}
    >
      <motion.div
        className="absolute inset-x-0 bottom-0 mx-auto h-[210px] w-[390px] rounded-[28px] bg-gradient-to-b from-rose-200 via-pink-200 to-rose-300 shadow-[0_30px_60px_rgba(244,114,182,0.25)]"
        animate={{ rotateX: open ? 8 : 0, y: open ? 10 : 0 }}
        transition={{ duration: 0.9, ease: "easeInOut" }}
        style={{ transformStyle: "preserve-3d" }}
      >
        <div className="absolute inset-0 rounded-[28px] border border-white/50" />
        <div className="absolute inset-x-0 bottom-0 h-[120px] rounded-b-[28px] bg-gradient-to-t from-rose-400/25 to-transparent" />
        <div className="absolute left-0 top-0 h-0 w-0 border-b-[210px] border-l-[195px] border-b-pink-300/90 border-l-transparent" />
        <div className="absolute right-0 top-0 h-0 w-0 border-b-[210px] border-r-[195px] border-b-pink-300/90 border-r-transparent" />
        <div className="absolute left-1/2 top-[24px] h-[150px] w-[300px] -translate-x-1/2 rounded-[22px] border border-white/70 bg-gradient-to-b from-white/95 via-rose-50 to-pink-100 shadow-inner" />
      </motion.div>

      <motion.div
        className="absolute left-1/2 top-[14px] h-0 w-0 -translate-x-1/2 origin-top border-l-[210px] border-r-[210px] border-t-[150px] border-l-transparent border-r-transparent border-t-pink-400 drop-shadow-[0_14px_22px_rgba(244,114,182,0.25)]"
        animate={{ rotateX: open ? 180 : 8, z: open ? 30 : 0 }}
        transition={{ duration: 1, ease: [0.22, 1, 0.36, 1] }}
        style={{ transformStyle: "preserve-3d", backfaceVisibility: "hidden" }}
      >
        <div className="absolute left-1/2 top-[-140px] h-0 w-0 -translate-x-1/2 border-l-[170px] border-r-[170px] border-t-[122px] border-l-transparent border-r-transparent border-t-pink-300/90" />
      </motion.div>

      <motion.div
        className="absolute left-1/2 top-[54px] flex h-[190px] w-[310px] -translate-x-1/2 items-center justify-center rounded-[24px] border border-white/80 bg-gradient-to-b from-white via-rose-50 to-pink-100 px-6 shadow-[0_18px_36px_rgba(255,255,255,0.55)]"
        animate={{
          y: open ? -55 : 0,
          rotateX: open ? -8 : 0,
          scale: open ? 1.02 : 1,
        }}
        transition={{ duration: 0.95, ease: [0.22, 1, 0.36, 1] }}
        style={{ transformStyle: "preserve-3d" }}
      >
        <div className="absolute inset-0 rounded-[24px] bg-[radial-gradient(circle_at_top,_rgba(255,255,255,0.9),_transparent_50%)]" />
        <div className="relative text-center">
          <motion.div
            animate={{ rotate: open ? 0 : [0, -4, 4, 0] }}
            transition={{ duration: 2.4, repeat: Infinity }}
            className="text-4xl"
          >
            💌
          </motion.div>
          <div className="mt-3 text-xl font-black tracking-wide text-rose-500">Birthday Card</div>
          <div className="mt-2 text-sm font-medium text-rose-400">mở thiệp để xem điều bất ngờ</div>
        </div>
      </motion.div>

      <motion.div
        className="absolute bottom-[-10px] left-1/2 h-8 w-[320px] -translate-x-1/2 rounded-full bg-rose-300/30 blur-xl"
        animate={{ scaleX: open ? 1.15 : 1, opacity: open ? 0.5 : 0.28 }}
        transition={{ duration: 0.8 }}
      />
    </div>
  );
}

export default function BirthdayCardPopoutCake() {
  const [opened, setOpened] = useState(false);
  const [showCake, setShowCake] = useState(false);
  const [bursts, setBursts] = useState([]);
  const [typed, setTyped] = useState("");
  const [showHint, setShowHint] = useState(true);
  const [showLetter, setShowLetter] = useState(false);
  const [typedWish, setTypedWish] = useState("");

  useEffect(() => {
    if (!opened) return;
    setShowHint(false);
    const t = setTimeout(() => setShowCake(true), 550);
    return () => clearTimeout(t);
  }, [opened]);

  useEffect(() => {
    if (!showCake) return;
    const message = "happy birthday Thức";
    let i = 0;
    const timer = setInterval(() => {
      setTyped(message.slice(0, i + 1));
      i += 1;
      if (i >= message.length) clearInterval(timer);
    }, 120);
    return () => clearInterval(timer);
  }, [showCake]);

  useEffect(() => {
    if (!opened) return;
    const timer = setTimeout(() => {
      setShowLetter(true);
    }, 5000);
    return () => clearTimeout(timer);
  }, [opened]);

  useEffect(() => {
    if (!showLetter) return;
    const wish = `Chúc mừng sinh nhật tuổi 24 của Thức nhé! 🎂

Em biết những ngày anh đi làm xa rất vất vả, nhiều lúc mệt mỏi cũng không biết dựa vào ai. Nhưng em mong rằng ít nhất trong ngày sinh nhật này, anh sẽ được thư giãn, được vui và cảm thấy thật trọn vẹn.

Mong anh luôn mạnh khỏe, công việc thuận lợi, mọi thứ suôn sẻ và lúc nào cũng giữ được nụ cười của mình.

Dù không ở cạnh anh trong ngày đặc biệt này, nhưng em vẫn luôn ở phía sau ủng hộ và thương anh rất nhiều.

Khi nào anh về, em sẽ tặng quà và cắt bánh bù cho anh sau nhé 🤍

Happy birthday, Thức! 🎉`;

    let i = 0;
    const timer = setInterval(() => {
      setTypedWish(wish.slice(0, i + 1));
      i += 1;
      if (i >= wish.length) clearInterval(timer);
    }, 24);

    return () => clearInterval(timer);
  }, [showLetter]);

  useEffect(() => {
    if (!showCake) return;

    const makeBurst = () => ({
      id: Math.random().toString(36).slice(2),
      x: rand(8, 92),
      y: rand(8, 42),
      particles: Array.from({ length: 18 }).map((_, i) => {
        const angle = (Math.PI * 2 * i) / 18;
        const dist = rand(32, 86);
        return {
          dx: Math.cos(angle) * dist,
          dy: Math.sin(angle) * dist,
          color: COLORS[Math.floor(Math.random() * COLORS.length)],
        };
      }),
    });

    setBursts([makeBurst(), makeBurst()]);
    const interval = setInterval(() => {
      setBursts((prev) => [...prev.slice(-5), makeBurst()]);
    }, 1100);
    return () => clearInterval(interval);
  }, [showCake]);

  const confetti = useMemo(
    () =>
      Array.from({ length: 34 }).map((_, i) => ({
        id: i,
        left: rand(0, 100),
        delay: rand(0, 2.4),
        duration: rand(3.3, 5.4),
        rotate: rand(-180, 180),
        size: rand(8, 16),
        color: COLORS[i % COLORS.length],
      })),
    []
  );

  return (
    <div className="relative flex min-h-screen items-center justify-center overflow-hidden bg-gradient-to-b from-rose-100 via-pink-100 to-sky-100 px-4 py-10">
      <div className="absolute inset-0 bg-[radial-gradient(circle_at_top,_rgba(255,255,255,0.95),_transparent_42%)]" />

      {showCake &&
        confetti.map((c) => (
          <motion.span
            key={c.id}
            initial={{ y: -40, opacity: 0 }}
            animate={{ y: [0, 760], x: [0, 22, -14, 12], opacity: [0, 1, 1, 0] }}
            transition={{ duration: c.duration, repeat: Infinity, delay: c.delay, ease: "easeIn" }}
            className="absolute rounded-sm"
            style={{
              left: `${c.left}%`,
              top: "-4%",
              width: c.size,
              height: c.size * 0.58,
              rotate: `${c.rotate}deg`,
              background: c.color,
            }}
          />
        ))}

      {bursts.map((b) => (
        <Firework key={b.id} burst={b} />
      ))}

      <div className="relative z-10 flex w-full max-w-5xl flex-col items-center">
        <motion.div
          initial={{ scale: 0.96, opacity: 0 }}
          animate={{ scale: 1, opacity: 1 }}
          transition={{ duration: 0.8 }}
          className="relative flex flex-col items-center"
        >
          <div className="scale-[0.82] md:scale-[0.94]">
            <Envelope open={opened} onOpen={() => setOpened(true)} />
          </div>

          <AnimatePresence>
            {showHint && !opened && (
              <motion.div
                initial={{ opacity: 0, y: 6 }}
                animate={{ opacity: 1, y: 0 }}
                exit={{ opacity: 0, y: -8 }}
                transition={{ duration: 0.45 }}
                className="pointer-events-none absolute -bottom-10 left-1/2 -translate-x-1/2 rounded-full bg-white/70 px-4 py-2 text-sm font-semibold text-rose-400 shadow-md backdrop-blur"
              >
                chạm vào thiệp
              </motion.div>
            )}
          </AnimatePresence>

          <AnimatePresence>
            {showCake && (
              <motion.div
                initial={{ y: 120, opacity: 0, scale: 0.62, rotateX: -18 }}
                animate={{ y: -12, opacity: 1, scale: 0.76, rotateX: 0 }}
                exit={{ opacity: 0 }}
                transition={{ duration: 1.15, ease: [0.22, 1, 0.36, 1] }}
                className="absolute bottom-[102px] left-1/2 z-20 -translate-x-1/2 md:bottom-[112px] [transform-style:preserve-3d]"
              >
                <motion.div
                  animate={{ y: [0, -16, 0], scale: [1, 1.025, 1], rotateZ: [0, -0.8, 0.8, 0] }}
                  transition={{ duration: 1.8, repeat: Infinity, ease: "easeInOut" }}
                  className="flex flex-col items-center"
                >
                  <div className="relative [transform-style:preserve-3d]">
                    <Tier width={150} height={60} colors={["#CDB4FF", "#BDE0FE"]}>
                      <Candle left="24%" />
                      <Candle left="49%" />
                      <Candle left="72%" />
                    </Tier>

                    <div className="-mt-3">
                      <Tier width={230} height={76} colors={["#FFC6DE", "#FFDDEB"]} />
                    </div>

                    <div className="-mt-4">
                      <Tier width={310} height={92} colors={["#FFE29A", "#FFB7C5"]} />
                    </div>

                    <div className="mx-auto mt-1 h-5 w-[340px] rounded-full bg-pink-200/55 blur-md" />
                  </div>
                </motion.div>
              </motion.div>
            )}
          </AnimatePresence>
        </motion.div>

        <div className="mt-8 h-20 flex items-center justify-center">
          <AnimatePresence>
            {showCake && (
              <motion.h1
                initial={{ opacity: 0, y: 18 }}
                animate={{ opacity: 1, y: 0 }}
                transition={{ duration: 0.8 }}
                className="px-3 text-center text-3xl font-black tracking-wide text-pink-600 md:text-5xl"
                style={{ textShadow: "0 4px 14px rgba(255,255,255,0.95)" }}
              >
                {typed}
              </motion.h1>
            )}
          </AnimatePresence>
        </div>

        <AnimatePresence>
          {showLetter && (
            <motion.div
              initial={{ opacity: 0, y: 40, rotateX: -20, scale: 0.94 }}
              animate={{ opacity: 1, y: 18, rotateX: 0, scale: 1 }}
              transition={{ duration: 0.9, ease: [0.22, 1, 0.36, 1] }}
              className="mt-6 w-full max-w-2xl px-4"
            >
              <div className="relative mx-auto rounded-[28px] border border-rose-200/70 bg-gradient-to-b from-white via-rose-50 to-pink-100 p-6 shadow-[0_24px_60px_rgba(244,114,182,0.18)] md:p-8">
                <div className="absolute inset-x-8 top-0 h-6 rounded-b-[18px] bg-white/55 blur-md" />
                <div className="mx-auto mb-5 h-2 w-24 rounded-full bg-rose-200/70" />
                <div className="whitespace-pre-line text-left text-[15px] leading-7 text-rose-700 md:text-base md:leading-8">
                  {typedWish}
                </div>
              </div>
            </motion.div>
          )}
        </AnimatePresence>
      </div>
    </div>
  );
}
